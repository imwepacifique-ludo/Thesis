# Meteorological Data — Preprocessing & Feature Engineering

This repository contains a Jupyter notebook that performs data preprocessing and feature engineering for the meteorological historical dataset located in `Dataset/meteo-historical-data.json`.

**Files of interest**
- `data_preprocessing_feature_engineering.ipynb` — Notebook with data loading, cleaning, and feature engineering steps.
- `Dataset/meteo-historical-data.json` — Raw data (source file expected). 
- `Dataset/processed_meteo_data.json` — Output created by the notebook after successful run.
- `artifacts/` — Contains saved scalers and encoders (e.g., `scaler.pkl`, `label_encoders.pkl`).
- `requirements.txt` — Python dependencies for the notebook.

**Prerequisites**
- Python 3.8+ recommended
- `git` (optional) and terminal access

**Create a virtual environment and install dependencies**

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

**Run the notebook locally**

1. Start Jupyter Notebook or JupyterLab:

```bash
jupyter notebook
# or
jupyter lab
```

2. In the browser, open `data_preprocessing_feature_engineering.ipynb` and run the cells in order. The notebook performs:
   - Robust loading of `Dataset/meteo-historical-data.json` (tries standard JSON and JSON Lines formats).
   - Datetime detection/parsing and index setting.
   - Missing-value handling (interpolation, median, forward/backfill fallback).
   - Outlier capping (IQR-based).
   - Temporal feature creation (year, month, hour, dayofweek, cyclical encodings).
   - Lag and rolling-window features for numeric variables.
   - Optional interactions (temperature × humidity) and wind vector components when fields exist.
   - Numeric scaling and persistence of scalers/encoders to `artifacts/`.
   - Saves processed JSON to `Dataset/processed_meteo_data.json`.

**Run the notebook non-interactively (execute all cells)**

You can run the notebook end-to-end with `nbconvert`:

```bash
jupyter nbconvert --to notebook --execute data_preprocessing_feature_engineering.ipynb --output executed_notebook.ipynb
```

**Notes & troubleshooting**
- If the notebook cannot find a datetime column, open the notebook and set the correct column name (search for the `time_cols` detection cell). The code tries to auto-detect common names but may require manual adjustment for custom schemas.
- If your JSON uses nested structures (objects within rows), you may need to flatten records before running; add a small pre-processing cell to expand nested dicts into columns using `pd.json_normalize`.
- If you have large data and running into memory limits, consider sampling or using chunked processing.

**Adjusting column names**
- The notebook uses common column names (`temperature`, `humidity`, `wind_speed`, `wind_direction`) to create interaction and vector features. If your dataset uses different names, update the corresponding checks inside the notebook before running.

**Reproducibility**
- The notebook saves `scaler.pkl` and `label_encoders.pkl` to `artifacts/` so trained models can reuse the same preprocessing pipeline.

**Contact / Next steps**
- After preprocessing, consider running exploratory data analysis (EDA), feature selection, and building forecasting/classification models depending on your thesis goals.

