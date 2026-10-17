# Freight Rate Prediction

Machine learning assessment by Mina Ibrahim.

## Approach

The notebook compares a distance-based baseline, Ridge regression,
Random Forest, MLP and CatBoost. The selected model is Random Forest
with geographic coordinates.

Validation uses two chronological development windows:

- Train January-August; evaluate September-October 2025.
- Train January-June; evaluate July-August 2025.

Both windows informed model selection; neither is an untouched final test.
Invalid non-positive weights are treated as missing. Imputation statistics
are fitted only on the training portion of each split.

## Setup

Create and activate a Python environment, then install:

```bash
python -m pip install -r requirements.txt
```

Place the supplied assessment files in `data/`:

- train_test.csv
- validation.csv
- validation_predictions_template.csv
- december_chart_inputs.csv

Keep the supplied `score.py` in the project root.
Original datasets are excluded from this repository.

## Run

```bash
python -m jupyterlab
```

Open `notebooks/02_final_workflow.ipynb`, restart the kernel,
and run all cells in order.

The notebook generates:

- outputs/validation_predictions.csv
- outputs/december_predictions.csv
- outputs/model_comparison_sep_oct.csv
- outputs/model_comparison_jul_aug.csv
- outputs/scorer_results/candidate_december.png

## Validate Output Files

```bash
python score.py \
  --predictions outputs/validation_predictions.csv \
  --december-predictions outputs/december_predictions.csv \
  --output-dir outputs/scorer_results
```

The supplied scorer validates file structure and generates the chart.
Hidden-set prediction metrics are calculated by Spotter.

## Selected-Model Results

| Development window | MAE ($) | RMSE ($) | R-squared |
|---|---:|---:|---:|
| September-October | 117.29 | 632.24 | 0.8284 |
| July-August | 121.85 | 628.11 | 0.8224 |

## Limitations

Extreme prices were retained and remain a source of large errors.
December is unseen in labeled training. The fixed December chart reflects
weekday effects, not validated December or holiday seasonality.
Its shaded area is decorative, not an uncertainty interval.

December coordinates are recovered from a checked labeled-data lookup.
No external geocoding is used.
