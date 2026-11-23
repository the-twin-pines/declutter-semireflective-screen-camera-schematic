# CPS printed-paper schematic restoration — v1 design and acceptance

Status: **design/intake only; no cleaning or validation run is claimed.**

Issue: [#5](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/issues/5).

This is a new **photographed printed-paper** input mode, not a claim that the
existing scrolled-HMI/ReTech 1712–1723 registration applies to these photos.
All 22 originals are preserved in the user's private
`Engineering / Schematic Cleanup / cps_paper_22_originals_private_2026-11-23.zip`
intake. The accompanying [SHA-256 manifest](../inputs/cps-paper-2026-11-23.tsv)
is metadata only. **These JPEGs are not yet GitHub assets**: the repository is
public and the old work explicitly excluded private source photos. Recording
the package's Library location does not let GitHub Actions fetch its bytes.

## What to remove — and what not to remove

| Camera or presentation artifacts to remove | Genuine drawing information to retain |
| --- | --- |
| Illumination gradients, camera shadows, color cast | Thin/light conductors, junction dots, non-joined crossings |
| Phone reflection, shiny sleeve/binder glare, clipped flash | Relay contacts, connectors, pin numbering, I/O references |
| Camera perspective, orientation, barrel distortion if measured | Original relative geometry and wire routing |
| Out-of-page tabletop, binding, phone watermark | Drawing's border, title block, revision, sheet references |
| Glare/shadow textures with independent negative evidence | Small-print notes, legends, technical annotations |

**Removal is evidence-conditional.** A black line is not a photograph artifact
just because it looks like a reflection. If the `SHOT ON MIRO C67` watermark
covers real marks, obtain their appearance from a second photograph or leave
them **UNRESOLVED**. Treat both overexposed white cores and saturated dark
shadows as absent information, not white paper. Avoid destructive 1-bit
thresholding in the master. Generate high-contrast/binarized *review*
derivatives only after checking the original greyscale evidence.

## Intended output per *verified* paper sheet

```text
pages/<observed-drawing-sheet-revision>/
  rectified-source.ppm        # high-resolution orientation/geometry-correct view
  readable.ppm                # printable paper-white/black-ink, fine detail retained
  contrast-preview.png        # optional non-authoritative display derivative
  source-map.tsv              # output regions/pixels -> contributing image(s), source coordinates
  uncertain.tsv               # glare, blur, clipped mark, unsupported join, text ambiguity
  witnesses.tsv               # registered line, dot, label, title/revision examples
  review.html                 # original-crop/clean-crop toggle and exact coordinates
```

The desired view resembles a legible engineering photocopy: straight paper,
near-white background, dark crisp thin wires/text, original symbols and notes.
No ghost images or camera glare. **No guessed electrical netlist, inferred
connection, rewritten label, or invented symbol** may be represented as a fact.

Retain a lossless, non-thresholded master and original evidence at native
resolution; any scaling should report effective sampling and interpolation.
The intended outputs are **one per sheet**. Multiple distinct sheets are not
one mosaic.

## Pipeline and independently checkable stages

1. **Admit** original JPEG SHA-256, size, decoded channel order/EXIF orientation,
   exact decoded bytes and source identities. Never change the original. Reuse
   fresh-output collision admission and explicit provenance, not ad-hoc overwrite.
2. **Classify logical sheets** from observed drawing number, revision, sheet
   number and unique terminal/wire features. Index candidates, don't assert
   equality from common manufacturer headers or repeated module legends.
   Identical files are deduplicated by hash; `1000002835.jpg` was posted twice.
3. **Rectify geometry**: observed page/frame corner candidates or verifiable
   internal fiducials, oriented coordinate frames and an admitted planar
   homography. Use independent held-out distinct printed landmarks to reject
   bad fits; edges/folds need local uncertainty. Do not align using just parallel
   wires, repetitive terminals, glare, the phone's fixed overlay or shadows.
4. **Estimate defects** in camera coordinates: low-frequency illumination,
   saturation/glare, reflections/shadows, blur, outside-paper/background and
   occlusion. Classify cause with evidence; annotate rather than erase when
   uncertain.
5. **Fuse only true same-page views**: find robust homography *per pair*;
   select the sharper, unclipped observation per output pixel/region, with
   seam handling on paper background, not through a conductor. Do not average
   a saturated white patch into real black ink. Retain selected or competing
   evidence with source coordinates, masks and confidence/support categories.
   If all views lack a mark, leave a visible unresolved patch.
6. **Enhance** on the rectified/fused master. Favor conservative background
   flattening and locally measured contrast, retaining anti-aliased thin strokes.
   Save both unenhanced rectified reference and enhanced display raster.
7. **Review and qualify** with the independent witness rules below; only then
   mark a sheet/region accepted. A pretty or photometrically consistent output
   is not evidence of electrical correctness.

### Multi-view correspondence

For output page point `p`, store inverse mappings `H_i^{-1}(p)` into each
source image `i`, including the chosen image ID and evidence class. A tile
or pixel supported by no unsaturated observation is **unknown**, not the mean
of bad pixels. Keep separate sheet groups if the drawing numbers or topology
conflict. Composition with multiple registered images should be rejectable by
an independent alignment verifier; never use the very same registration code
as its sole oracle.

## Witness and review ledger

Every claim cites `original SHA + source rectangle + page rectangle + stage
parameters + candidate digest`. Critical observations should include:

- Thin conductor through a dark shadow; tiny junction dot vs plain crossing.
- Contact's normally-open/normally-closed symbol and relay number.
- Off-sheet arrow/coordinate, e.g. wire cross-reference.
- Adjacent small text that differs by one glyph: `24V` vs `2AV`, `0` vs `O`,
  pin `1` vs `7`, leading zero and punctuation.
- Title-block drawing number, revision, sheet number.
- Flash-covered region that another photograph may or may not recover.

Each witness status is `PASS`, `FAIL`, or `UNRESOLVED` with a reason. PASS
requires an independent readable original or corroborating true same-page
view; synthetic validity and reconstructed appearance alone do not grant it.
A single lost critical stroke or added false electrical junction fails that
region even if global pixel similarity is high.

### Required negative controls

Destroy one genuine wire; insert an artificial connecting dot at a crossing;
swap a pin digit; erase faint text; relabel one original SHA; shift one overlap;
attach the wrong sheet as a second view; replace a source-map coordinate;
clip all source observations under a flash hotspot. The independent reviewer
must detect these changes or label the region unresolved. In particular,
the all-flashed case **must not** produce a verified recovered conductor.

## Synthetic training and validation — authorized

Generate **clean artificial drawings as actual ground truth**, then render
multiple simulated camera views of each same clean sheet. Vary controlled
perspective/tilt, rotation, translation/crop, flash ellipses with clipping,
soft broad specular bands, exposure/illumination gradients, cast shadows,
defocus, grain/JPEG damage and slight print line-width differences. Include
subtle one-pixel wires, small labels, junction dots, nonjoined crossings and
non-diagram marks. Store:

```text
synthetic/<diagram-id>/
  target-clean.ppm
  scene-metadata.tsv          # exact page content seed, warp and camera settings
  view-N.ppm
  view-N-validity-mask.ppm    # saturated/occluded/unobserved pixels
  view-N-to-page.tsv          # declared geometric transform
```

This is a test-data boundary. Existing Idriç production must retain sole
ownership of admitted photo geometry, source/semantic provenance and processing
checks. Synthetic renderer may be isolated as a test fixture generator and
must publish its seeds and exact target image identity. For split hygiene,
**never distribute different views of the same diagram across train and test**.
Hold out entire diagram layouts, glyph/terminal combinations and distortion
seeds; use known-clean synthetic targets only for quantitative measures. Include
intentionally wrong registrations and destructive corruption examples.

A synthetic benchmark verifies recovery of known pixels/features when a
non-saturated view exists; when all views are clipped it verifies honest
`UNRESOLVED`, **not** inferred ink. It is a supplement to—not replacement
for—real-original eyewitness review and topology-safe acceptance.

## Inventory and first practical scope

The attached batch spans overview/system blocks, operator interface diagrams,
register control board, physical terminal-layout sheets, RDC1/RDC2 drives,
safety relay/ESTOP and related wiring. These are **different kinds of pages**:
`2831`–`2833` appear to be separate system-block/PLC overviews, while
`2844`–`2845` depict corresponding motor-drive areas; these are provisional
observations, not accepted page joins. Classification requires reading
page identifiers from originals and confirming independent landmarks.

Start with one comparatively readable complete sheet (for geometry/background
baseline), then an overlapping pair with disjoint glare, then a deliberately
unrecoverable flash patch. Log PASS/FAIL/UNRESOLVED for each; do not claim
full-batch cleaning until the remaining 22 are actually processed.

## Boundary with existing work

Prior ReTech HMI photos `1712`–`1723` and their assembly plans are distinct.
Keep their existing `1718 24V-DC` preservation regression open. Reuse the
current `SchematicRaster`, `Cleanup`, `PhotoReview`, provenance and independent
`VerifyCoverage` facilities **only where their contracts fit** this new
printed-paper pipeline; do not pretend they already provide camera rectification,
flash-aware fusion, or real-page acceptance.
