# AutoValue — Used Car Price Prediction

## Overview

AutoValue is an end-to-end machine learning project for predicting used-car listing prices from vehicle characteristics.

The project follows an iterative modelling approach: beginning with a simple linear regression baseline, progressively introducing additional features, and then moving toward reusable preprocessing and modelling pipelines.

The current version focuses on vehicles priced between **$1,000 and $100,000**, representing the mainstream used-car market.

The original dataset contains more than **760,000 vehicle listings**, providing a large real-world dataset for exploring regression, feature engineering, categorical encoding, model evaluation, and machine learning pipeline design.

---

## Project Objectives

The main objectives of AutoValue are to:

- explore and clean a large real-world vehicle dataset
- identify data-quality issues and extreme target values
- engineer useful vehicle-related features
- establish interpretable regression baselines
- incorporate numerical and categorical features
- build reproducible preprocessing and modelling pipelines
- compare optimization approaches using consistent evaluation metrics
- visualize prediction behaviour and model limitations
- progressively evaluate more powerful regression models

---

## Dataset

The dataset contains approximately **762,000 used-car listings** with information including:

- Manufacturer
- Model
- Year
- Mileage
- Engine
- Transmission
- Drivetrain
- Fuel Type
- Exterior and Interior Color
- Accident History
- Ownership Information
- Seller Information
- Driver Ratings
- Price

The prediction target is:

`price`

### Modelling Scope

Exploratory analysis revealed that the extreme upper end of the price distribution contains a mixture of potentially legitimate collector vehicles and clearly implausible listings.

AutoValue v1 therefore focuses on the mainstream used-car market:

`$1,000 <= price <= $100,000`

Vehicles outside this range are excluded from the initial modelling population rather than automatically being classified as invalid observations.

After applying the modelling criteria and removing observations with missing mileage, approximately **751,000 listings** remain for model development.

---

## Project Workflow

### 1. Exploratory Data Analysis

The dataset is inspected to understand:

- dataset dimensions
- feature types
- missing values
- descriptive statistics
- price distribution
- mileage distribution
- extreme observations
- relationships between vehicle characteristics and price

### 2. Data Preparation

The modelling dataset is prepared by:

- defining the target price range
- handling missing mileage
- creating vehicle age
- investigating extreme price observations
- separating features and target variables
- creating reproducible train/test splits

### 3. Baseline Linear Regression

The first model uses only:

`mileage`

This establishes a simple and interpretable baseline against which subsequent models can be compared.

### 4. Multiple Linear Regression

Vehicle age is added to determine whether depreciation information improves predictive performance.

Features:

- Mileage
- Vehicle Age

### 5. Manufacturer Encoding

Manufacturer is incorporated using one-hot encoding so that brand information can be used without introducing an artificial numerical ordering between manufacturers.

Features:

- Mileage
- Vehicle Age
- Manufacturer

Manufacturer information produced the largest improvement among the initial linear-regression experiments.

### 6. High-Cardinality Model Investigation

The vehicle `model` feature contains more than **11,000 unique values**.

The frequency distribution was investigated before encoding because thousands of vehicle models occur only a small number of times.

Rare vehicle models were grouped to reduce categorical cardinality, and the resulting model-level experiment was compared with the simpler manufacturer-based model.

The experiment demonstrated that introducing a higher-cardinality feature does not necessarily improve generalization.

### 7. Reusable Scikit-learn Pipeline

The strongest linear feature configuration was converted into a reusable scikit-learn pipeline using:

- `ColumnTransformer`
- `OneHotEncoder`
- `Pipeline`
- `LinearRegression`

The pipeline ensures that preprocessing and model training are performed as a consistent workflow and that preprocessing is learned from the training data.

### 8. Gradient-Based Linear Regression

A second linear model was trained using `SGDRegressor` to evaluate gradient-based optimization on the same prediction problem.

Numerical features were standardized using `StandardScaler` before SGD training because gradient-based optimization is sensitive to differences in feature scale.

The experiment demonstrates the distinction between:

- the **model** being optimized
- the **optimization algorithm** used to estimate its parameters

`LinearRegression` and `SGDRegressor` produced very similar predictive performance, showing that changing the optimization method alone does not overcome the limitations of the underlying linear model.

### 9. Prediction Diagnostics

An actual-versus-predicted visualization was used to examine model behaviour beyond aggregate evaluation metrics.

The analysis revealed:

- systematic underprediction of many high-priced vehicles
- substantial prediction spread for lower-priced vehicles
- some unrealistic negative price predictions
- patterns suggesting that vehicle pricing relationships are not completely linear

These observations motivate the future evaluation of nonlinear regression models.

---

## Current Model Results

| Model | Features | MAE | RMSE | R² |
| --- | --- | ---: | ---: | ---: |
| Baseline Linear Regression | Mileage | $10,173.71 | $13,728.05 | 0.3065 |
| Multiple Linear Regression | Mileage + Vehicle Age | $9,984.51 | $13,564.36 | 0.3230 |
| Manufacturer Linear Model | Mileage + Vehicle Age + Manufacturer | **$8,547.31** | $11,802.52 | 0.4874 |
| SGD Linear Model | Mileage + Vehicle Age + Manufacturer | $8,567.68 | **$11,799.62** | **0.4877** |

The manufacturer feature produced a substantial improvement over the numerical-only models.

The SGD and ordinary least-squares models achieve nearly identical performance. This is expected because both models still represent a linear relationship between the input features and vehicle price, despite using different optimization approaches.

The small numerical differences between their metrics should therefore not be interpreted as evidence that one approach is substantially more predictive than the other.

---

## Evaluation Metrics

### Mean Absolute Error (MAE)

MAE measures the average absolute difference between predicted and actual vehicle prices.

It provides an intuitive interpretation of approximately how many dollars the model is wrong by on average.

Lower values are better.

### Root Mean Squared Error (RMSE)

RMSE gives greater weight to large prediction errors because errors are squared before averaging.

It is useful for identifying models that occasionally make particularly large mistakes.

Lower values are better.

### R² Score

R² measures the proportion of variation in vehicle prices explained by the model.

Higher values generally indicate greater explanatory power, but R² should **not** be interpreted as prediction accuracy.

---

## Key Findings So Far

The experiments have produced several useful observations:

- Mileage has a negative relationship with vehicle price.
- Vehicle age provides additional predictive information beyond mileage alone.
- Manufacturer is a strong categorical predictor of vehicle price.
- Adding more features does not automatically improve generalization.
- High-cardinality categorical features require careful preprocessing.
- Feature scaling becomes particularly important when using gradient-based optimization.
- Different optimization algorithms can produce similar results when fitting the same model class.
- Diagnostic plots reveal model limitations that cannot be understood from MAE, RMSE, and R² alone.
- The remaining prediction patterns suggest that nonlinear relationships should be investigated.

---

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook
- Git
- GitHub

---

## Project Structure

```text
autovalue/
│
├── data/
│   └── raw/
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_modeling.ipynb
│   └── 03_model_pipeline.ipynb
│
├── src/
│
├── .gitignore
├── README.md
└── requirements.txt
```

The raw dataset is excluded from version control and should be obtained separately.

---

## Current Status

AutoValue currently includes:

- exploratory data analysis
- modelling-data preparation
- feature engineering
- baseline linear regression
- multiple linear regression
- categorical feature encoding
- high-cardinality feature investigation
- reusable scikit-learn preprocessing pipelines
- gradient-based regression with feature scaling
- MAE, RMSE, and R² evaluation
- actual-versus-predicted diagnostic visualization

The project is under active development.

---

## Planned Improvements

Future iterations will investigate:

- nonlinear regression models
- decision-tree-based regression
- ensemble methods
- additional vehicle features
- model tuning and validation
- residual and segment-level error analysis
- final model selection
- model persistence
- deployment through a prediction interface

These components will be moved from planned to completed as they are implemented and evaluated.