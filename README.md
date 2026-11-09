# Compact SVS Crop Color Evaluation

Research repository for an end-of-studies project on compact SVS acquisition and
non-destructive crop color evaluation.

The project studies pepper, lettuce, and zucchini images acquired with different
cameras and devices. The intended processing chain is:

1. **Color correction** to reduce device and camera bias.
2. **Data-quality filtering** to remove bad pixels and retain valid, saturated
   pixels.
3. **CNN-based color evaluation** that predicts an ordinal decimal score from
   1 to 9 for ripeness and visual quality.

The repository is organized as a research archive: notebooks contain the
experiments, while reports, presentations, and figures document the acquisition
choices, preprocessing comparisons, model variants, and ablation studies.
Original datasets are not included because they are large and may contain
institutional or proprietary material.

## Repository layout

```text
data/
  raw/                         # Local acquisition exports; not committed
  processed/                   # Generated datasets; not committed
figures/                       # Exported plots and diagnostics
notebooks/
  00_overview/                # Pipeline and supervisor-facing overview
  01_acquisition_quality/     # DNG/TIFF, filtering, and acquisition studies
  02_color_correction/        # CC vs NOCC and device-bias comparisons
  03_crop_scoring/            # Pepper, lettuce, and zucchini scoring pipelines
  04_experiments_ablations/   # CNN, color-space, encoding, and loss ablations
outputs/                       # Local metrics, checkpoints, and predictions
reports/
  technical/                  # Documents, spreadsheets, and acquisition notes
  presentations/              # Progress and results presentations
```

## Research questions

- How stable are color measurements across cameras, devices, and acquisition
  conditions?
- Does color correction improve generalization over uncorrected (NOCC) data?
- Which pixel-quality filters and saturation rules improve downstream learning?
- Which representation performs best: RGB, sRGB, linear/ACES encodings, LAB,
  HSV, or combined feature stacks?
- Which CNN architecture, ordinal/regression objective, and preprocessing
  combination produces the most reliable 1–9 quality score?

## Running a notebook

1. Create a local Python environment with the image, numerical, plotting, and
   deep-learning packages used by the selected notebook.
2. Put the source acquisitions under `data/raw/` or update the dataset root in
   the notebook configuration cell.
3. Run acquisition and quality checks before color-correction experiments.
4. Run crop-specific scoring notebooks and save generated tables/checkpoints
   under `outputs/`.
5. Record the dataset version, device, correction method, filter, split, random
   seed, and model configuration for every comparison.

Most notebooks currently contain historical absolute paths from the original
development environment. These paths must be updated locally before execution.
Rendered notebook outputs were removed during repository preparation to keep the
source archive lightweight; representative exported plots are retained in
`figures/diagnostics/`.

## Experiment map

| Area | Main comparisons |
|---|---|
| Acquisition | DNG versus corrected TIFF, tool and device comparisons |
| Correction | Color-corrected (CC) versus non-corrected (NOCC) images |
| Quality | Bad-pixel rejection, saturation filtering, and statistical LAB filtering |
| Crops | Pepper, lettuce, and zucchini color/ripeness scoring |
| Models | CNN regression, ordinal heads, dual heads, composite losses |
| Ablations | Color spaces, RAW encodings, backbone depth, feature branches, fine-tuning |

## Reproducibility

Use fixed seeds and grouped or stratified folds where applicable. Do not compare
metrics across notebooks unless the dataset, split protocol, preprocessing,
target definition, and evaluation metrics are identical. Keep raw data and
model checkpoints outside Git; commit only lightweight metadata and reproducible
analysis code.
