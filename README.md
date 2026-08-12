# AutoValue — Used Car Price Prediction

## Overview

AutoValue is a machine learning project for predicting used-car listing prices from vehicle characteristics.

The project follows an iterative modelling approach, beginning with a simple linear regression baseline and progressively introducing additional features to measure their impact on predictive performance.

The initial version focuses on vehicles priced between **$1,000 and $100,000**, representing the mainstream used-car market.

The project currently uses a dataset containing more than **760,000 vehicle listings**.

---

## Project Objectives

The main objectives of this project are to:

- explore and clean a large real-world vehicle dataset
- identify data-quality issues and extreme target values
- engineer useful vehicle-related features
- establish a simple regression baseline
- incorporate numerical and categorical features
- compare models using consistent evaluation metrics
- understand which vehicle characteristics contribute most to price prediction

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

### 3. Baseline Linear Regression

The first model uses only:

`mileage`

This establishes a baseline against which subsequent models can be compared.

### 4. Multiple Linear Regression

Vehicle age is added to determine whether depreciation information improves prediction performance.

### 5. Categorical Feature Encoding

Manufacturer is incorporated using one-hot encoding so that brand information can be used by the regression model without introducing an artificial numerical ordering.

---

## Current Model Results

| Model | Features | MAE | RMSE | R² |
|---|---|---:|---:|---:|
| Baseline Linear Regression | Mileage | $10,173.71 | $13,728.05 | 0.3065 |
| Multiple Linear Regression | Mileage + Vehicle Age | $9,984.51 | $13,564.36 | 0.3230 |
| Manufacturer Model | Mileage + Vehicle Age + Manufacturer | **$8,547.31** | **$11,802.52** | **0.4874** |

Adding manufacturer information produced the largest improvement so far, increasing R² from **0.3230 to 0.4874**.

These results are preliminary and will change as additional features and models are introduced.

---

## Evaluation Metrics

### Mean Absolute Error (MAE)

MAE measures the average absolute difference between predicted and actual vehicle prices.

It provides an intuitive interpretation of approximately how many dollars the model is wrong by on average.

### Root Mean Squared Error (RMSE)

RMSE gives greater weight to large prediction errors and helps identify models that make particularly large mistakes.

### R² Score

R² measures the proportion of variation in vehicle prices explained by the model.

R² should not be interpreted as prediction accuracy.

---

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook

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
│   └── 02_modeling.ipynb
│
├── src/
│
├── .gitignore
├── README.md
└── requirements.txt