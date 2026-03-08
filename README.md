# Chihuahua vs Muffin - Our Data-Centric Workflow

This repository documents our team workflow for improving classifier accuracy through **data work** in 3LC.What We Actually Changed

- We kept the model setup fixed (ResNet-18 from scratch as required).
- We did **not** do architecture tuning or pretrained-model tricks.
- We mainly improved results through:
  - iterative labeling in the 3LC dashboard,
  - label correction and ambiguity handling,
  - targeted sample weighting,
  - small training-setting checks (epochs, learning rate) to validate baseline stability.

In short: performance gains came from **better data decisions**, especially labeling strategy.

## Repository Files

- `register_tables.py` - registers local data into 3LC `train`/`val` tables.
- `train.py` - trains model, logs predictions/confidence/embeddings to 3LC, saves `best_model.pth`.
- `predict.py` - runs test inference and creates `submission.csv`.
- `REPORT.md` - full train-by-train methodology and results timeline.

## Data Layout

```text
data/
  train/
    chihuahua/
    muffin/
    undefined/
  val/
    chihuahua/
    muffin/
  test/            # flat folder
```

## Run

```bash
python register_tables.py
python train.py
python predict.py
```

Outputs:

- `best_model.pth`
- `submission.csv`

## How We Reached Peak Accuracy

Our highest gains came from this loop:

1. Train on current labels.
2. Inspect confidence + embedding clusters in 3LC.
3. Label uncertain/decision-boundary samples first.
4. Correct high-confidence label mismatches.
5. Set ambiguous samples to undefined (`weight = 0`).
6. Apply targeted weights to uncertain regions and retrain.

We also tested epoch/lr variations, but those gave smaller gains than labeling and data cleaning strategy.

For complete details and all run-level scores, see `REPORT.md`.