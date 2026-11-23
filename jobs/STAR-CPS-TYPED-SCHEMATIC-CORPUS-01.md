# STAR-CPS-TYPED-SCHEMATIC-CORPUS-01 — plan and allocation

**Status:** STAR plan complete; SUN implementation and testing **NOT_RUN**.
**Date:** 2026-11-23
**Emitted operative SUN job:** [SUN-CPS-TYPED-SCHEMATIC-CORPUS-01, issue #8](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/issues/8)
**Existing scope:** [paper-photo #5](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/issues/5), [paper mosaic #2](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/issues/2), draft PRs [#1](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/pull/1), [#3](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/pull/3), [#6](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/pull/6).

## Decision and evidence

The central task is not to make random lines look like a schematic; it is to
generate an original **typed electrical drawing and its exact graphical truth**,
then compare its photographic reconstructions against that independently
recorded truth. This permits controlled experiments where the correct answer
is known and deliberately wrong electrical connections can be tested.

At observed revision:

- PR #1 already supplies first-party `SchematicRaster.idric`, `PaperMesh.idric`,
  `PaperReplay.idric`, `PaperMeshTests.idric`, source-hash admission, a
  synthetic-only pinned Idriç/Chez workflow, and strict unknown gutters.
  `PaperMesh` models manually measured geometric triangles and exact
  source-pixel replay; it expressly **does not** model an electrical circuit or
  prove that a physically correct drawing was assembled.
- PR #3 is a standalone C11 **experimental** projective/multi-photo mesh
  reference, and its own PR description excludes measured geometric accuracy,
  verified photographic joins and first-party Idriç production integration.
  Reconcile its tests; do not replace the accepted Idriç route with C.
- PR #6 indexes 22 private CPS paper photographs by digest/dimensions and
  proposes page-aware cleaning and validation. The images themselves remain
  outside the public repository. Duplicate posting of `1000002835.jpg` is
  not a separate capture.
- Previous synthetic mesh fixtures are tiny source-pixel arrays. They
  validate byte identity and parser/geometry conditions, not missing
  conductors, false junctions, contact semantics or text identity.

Hence **source replay, geometric fidelity, and electrical fidelity are three
different claims**. None implies the other two. Real photographed-paper
acceptance is a fourth, independent evidence category.

## STAR architecture allocation

| Boundary | Decided requirement | SUN latitude |
| --- | --- | --- |
| Semantic ground truth | First-party, typed Idriç schematic model, separate graph incidence from drawn ink | Choose precise data constructors, admission proofs and simplest complete symbol subset |
| Clean drawing | Reproducible lossless P6 master derived from typed layout; symbol/mark witness manifest; no neural/OCR synthesis | Choose bounded rasterizer, glyphs, line widths and integer arithmetic |
| Camera observations | Deterministic multiple views of each sheet, projective/crop/illumination/glare/blur and exact coverage masks | Choose robust fixed-point transforms and simulator internals; publish seed/parameter receipts |
| Independent oracle | Verify geometry and circuit topology without relying exclusively on output source maps or the generator's own raster routine | Choose separate checks and explicit adversarial mutants |
| Incomplete evidence | Type and track observed, clipped, unobserved, conflicting and inferred **separately** | Choose witness granularity and uncertainty report interface |
| Privacy | Commit only synthetic original models, fixtures and receipts; not real user camera photos | Choose compact public fixture formats |
| Acceptance | All source hashes/shape/collision guards; zero tolerated fabricated junction/terminal identity in covered high-value witnesses; correct UNRESOLVED where no observation exists | Calibrate *measured* geometric tolerances and determine per-feature positive/negative coverage |
| Provenance | Keep source ID and sample coordinates; independent replay is necessary but never sufficient | Integrate with existing `PaperMesh` and PR #3 only after comparing contracts |

### Model boundary worth testing, not a presumed compiler proof

Prefer types that distinguish `NetId`, `DeviceId`, `TerminalId`,
`WireId`, `PagePoint`, `CameraPoint`, `PhysicalJunction`,
`PassWithoutConnection`, `GlyphLabel`, and source identity. A line
crossing at equal drawing coordinates must not automatically merge two nets.
A conductor is a semantic network assertion *and* a drawing primitive,
not a pixel color. Exact original incidence can be stored independent of
the drawing while checked construction proves internal references exist;
arbitrary user-supplied IDs still need validated admission. The SUN owner
must test actual Idriç support before claiming type-level guarantees.

Domain outline of initial example:

```text
Sheet A: 24V supply ── STOP (NC) ── START (NO) ── K1 coil ── 0V
           |       separate perpendicular wire crossing (NO DOT)
           |       selected branch with true connected junction (DOT)
Sheet B: PLC X41 terminals, wire colors, off-page arrow, symbol/label distinctions
Sheet C: a visually similar PLAN vs ELEVATION pair; intentionally invalid stitching
```

This ASCII is a *description*, not the ground truth file format or a
schematic whose connectivity is inferred from spacing. SUN owns an actual
typed source model and renderer so the tested artifacts are not pictures
invented without source semantics.

## SUN execution and evidence order

1. Preflight exact GitHub heads; read current Idriç style and compiler pin,
   `PaperMesh`, `PaperReplay`, test workflow and PR #3. No duplicate owner.
2. Model circuit connectivity and mark geometry, with checked constructors.
   Decide how semantic truth is serialized and independently inspected.
3. Render at least one full clean printed-paper schematic in native P6 through
   Idriç; exercise fine conductor, true junction, no-join crossing, NO/NC,
   terminal numbers and potentially confusable small lettering.
4. Create independently reproducible distorted views plus exact truth masks,
   including complementary glare and an *unrecoverable* saturated region.
5. Run separate positive and negative controls. A removed thin wire, added
   junction, shifted registration or swapped pin must not silently PASS.
   Nonvacuous checks reject an empty/all-grey accepted result.
6. Put only small seed-stable public synthetic fixtures, types, commands,
   output checksums and truthful acceptance receipts on a draft branch.
   Record compiler revision, runtime, tests, source digests, first failures.
7. SUN returns its decision/evidence, and **complete, bounded EARTH jobs**
   with nonoverlapping ownership: (a) typed symbol/model hardening;
   (b) virtual camera/registration and data coverage; (c) independent
   semantic/geometry oracle; (d) private real-paper pilot *only after* the
   synthetic oracle qualifies. SUN may consolidate these when justified.
   Each job must include exact prerequisite heads/inputs, files,
   acceptance controls, independent evidence and stop conditions.

**MOON:** NONE until all prerequisites are verified. Mechanical corpus
expansion may then be delegated with a named exact passing head and fixed
formats; it cannot change semantic truth or grant geometric/photographic PASS.

## Explicit exclusions / stop conditions

- No claim that the 22 CPS photographs have been cleaned, geometrically
  registered or electrically verified merely because synthetic drawings work.
- No public upload of source CPS photos, photographed diagrams or private
  derivative crops without a separate explicit publication decision.
- No automatic netlist inference from pixel intersections; no neural
  replacement of saturated/missing strokes treated as observed evidence.
- No false PASS from `PaperReplay` or from the source generator checking
  itself; independent geometry/topology witnesses must be present.
- No merge, PR closure, release or silent patching of unrelated ReTech
  1718/24V-DC preservation failure.
- If a required Idriç semantic type or backend facility is unsupported,
  record a reproducible BLOCKED outcome and surface the smallest language
  capability issue. Do not silently reimplement the entire authoritative
  pipeline in another language.

## Emitted SUN job

**[Issue #8](https://github.com/the-twin-pines/declutter-semireflective-screen-camera-schematic/issues/8)**
contains the full self-contained operative instructions, acceptance matrix,
fixtures, required proof of execution, GitHub head preflight, and downstream
EARTH/MOON handoff. Issue creation does not automatically execute the job.
