# Solar Power Generation Forecasting Using Machine Learning

A KDD-based predictive analytics project that explores solar power generation data and compares regression models for estimating plant-level AC power output.

## Project Overview

This project uses generation records and environmental sensor measurements from two solar plants in India. It demonstrates the Knowledge Discovery in Databases (KDD) workflow:

1. Data selection
2. Data cleaning and preprocessing
3. Data integration
4. Exploratory data analysis (EDA)
5. Feature transformation and selection
6. Chronological train/test split
7. Regression model training
8. Model evaluation
9. Feature-importance analysis and interpretation

**Target variable:** `AC_POWER` aggregated to plant/timestamp level.

> **Important:** Although the project uses the term forecasting, the notebook predicts AC power for timestamps with corresponding weather measurements. Since it uses contemporaneous weather readings and the dataset covers only about 34 days, it is a learning/research experiment—not a validated long-term or operational forecasting system.

## Dataset

**Source:** [Solar Power Generation Data — Kaggle](https://www.kaggle.com/datasets/anikannal/solar-power-generation-data)

The notebook requires these four CSV files:

- `Plant_1_Generation_Data.csv`
- `Plant_1_Weather_Sensor_Data.csv`
- `Plant_2_Generation_Data.csv`
- `Plant_2_Weather_Sensor_Data.csv`

Generation data is recorded at inverter level, while weather data contains plant-level sensor readings. The notebook aggregates inverter outputs by plant and timestamp, aggregates duplicate sensor readings where needed, and joins the data using plant and timestamp.

**Dataset redistribution:** The CSV files are not included in this repository. Download them from the original Kaggle source and review the dataset page's licence/terms before redistributing the data.

## Models and Evaluation

The notebook compares:

- Linear Regression
- Ridge Regression
- Random Forest Regressor

Evaluation metrics:

- **MAE (Mean Absolute Error):** average absolute prediction error.
- **RMSE (Root Mean Squared Error):** gives greater weight to larger errors.
- **R² (Coefficient of Determination):** measures how much target variance is explained on the test set.

The data is split chronologically, with the latest 20% of unique timestamps held out for testing. The notebook generates EDA plots, an evaluation table, actual-versus-predicted plots, and Random Forest feature importance. **Results are produced when you run the notebook; no performance scores are claimed in advance.**

## Features and Leakage Prevention

The primary model uses:

- Solar irradiation (`IRRADIATION`)
- Ambient temperature (`AMBIENT_TEMPERATURE`)
- Module temperature (`MODULE_TEMPERATURE`)
- Plant ID
- Time-derived features

Direct generation-related variables such as `DC_POWER`, `DAILY_YIELD`, and `TOTAL_YIELD` are excluded from the primary feature set to reduce target leakage.

## Run in Google Colab

1. Download or clone this repository, or download the notebook file.
2. Open [Google Colab](https://colab.research.google.com/).
3. Select **File → Upload notebook** and upload `Solar_Power_Generation_Forecasting_KDD.ipynb`.
4. Download the four CSV files from the [Kaggle dataset page](https://www.kaggle.com/datasets/anikannal/solar-power-generation-data).
5. Run the notebook cells from top to bottom. When the data-upload cell appears, select **all four CSV files together**. Do not upload only the ZIP file unless you first extract it and select the CSV files.
6. Inspect the generated plots and metrics, then complete the results/discussion section using your actual outputs.
7. Save or download the executed notebook if you want to preserve outputs in your copy.

If a file-not-found error appears, check that the uploaded filenames include the plant number and data type, such as `Plant_1_Generation_Data.csv`.

## Run Locally

Requirements: Python 3.10 or later is recommended.

1. Clone or download this repository.
2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Create a `dataset/` folder beside the notebook and place all four CSV files in it.
4. Open the notebook in Jupyter and run the cells in order.

## Repository Structure

```text
solar-power-generation/
├── Solar_Power_Generation_Forecasting_KDD.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── dataset/
    └── .gitkeep
```

The notebook also supports uploading the four CSV files directly in Google Colab, so the `dataset/` folder is not needed for Colab execution.

## Limitations

- The dataset covers approximately 34 days and two plants, so it does not establish year-round generalization.
- The available weather fields include irradiation and temperatures; humidity and wind speed are not included.
- Contemporaneous weather observations are used as predictors. A true future forecast would require forecast weather inputs or carefully designed lagged features.
- Random Forest feature importance indicates how the fitted model uses predictors; it does not establish causation.
- Model performance should be interpreted using the chronological test results generated by the notebook.

## References

- [Kaggle — Solar Power Generation Data](https://www.kaggle.com/datasets/anikannal/solar-power-generation-data)
- [scikit-learn documentation](https://scikit-learn.org/stable/)
- [Pandas documentation](https://pandas.pydata.org/docs/)
