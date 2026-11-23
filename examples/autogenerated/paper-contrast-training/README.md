# Fit a contrast threshold on synthetic paper diagrams

This experiment fits one integer threshold for marking visible ink in two aligned observations. It runs on a standard GitHub-hosted Ubuntu CPU, using generated drawings only. It is a measurement of the training path and a baseline for the schematic project in issue #5.

## Type-system sketch

A training example pairs a clean ink/background label with an optional contrast measurement. The program receives two observations of the same page coordinate. Fully clipped centers carry no measurement. From each usable view it measures the center against its four immediate neighbors, ignoring saturated neighbors. It takes the strongest available evidence across views. The clean generator supplies truth only for scoring.

`fit_threshold : List Example → Int` minimizes three times the number of missed visible ink samples plus the number of invented marks. The finite thresholds are 1 through 64 inclusive. Equal costs choose the smaller threshold. The fixed comparison uses 32 brightness levels. The abstention rule is fixed independently of training.

## Generator and evaluation

Each of 80 underlying 24 × 24 line drawings has a distinct vertical and horizontal line position, a contact gap and a dot. Two deterministic camera views alter lighting, ink contrast, noise, and the position of saturated flash. Both views have known common coordinates. Every fifth sheet is withheld from fitting. Counts cover interior pixels only, permitting a complete neighborhood in each view.

The executable emits the chosen model coefficient, split sizes, and train/held-out confusion counts as TSV. Completely clipped pairs are counted separately. A useful run must contain ink and background observations, include abstentions, and choose a threshold whose training cost is at most the fixed comparison. The held-out result is measured without a required improvement. This pilot does not estimate camera geometry or qualify real CPS photographs.

## Execution

The workflow compiles the Idriç source on the maintained Chez backend at compiler pin `2bce7677f513d711875e978bd6f5ad28b9e54d58`. Each run retains its exact source revision, model, validation counts and fit time.
