# Used Car Price Prediction · Mercedes-Benz Hackathon

Machine learning model to predict the price of second-hand cars, built for a hackathon organized by **Mercedes-Benz** in November 2022.

**Team 20 (GAMMA):** Brian Giannoni and Gorka Urzelai

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)

## The challenge

The second-hand car market moves a lot of money but has little research behind it. The goal was to use the data provided by Mercedes-Benz to:

- **Analyze trends** in the used car market.
- **Predict the price** of used cars from their features.

Our solution had two parts:

1. **Price prediction model** in Python and Jupyter (this repo).
2. **Data visualization dashboard** in Power BI, built on top of the cleaned data exported by the notebook.

## The data

The organizers provided a dataset with several sources (`dataset.zip`):

| File | Content |
|---|---|
| `Anuncios_usados.csv` | 268,252 used car ads with 23 columns (maker, model, year, mileage, engine, fuel, size, price in USD...) |
| `Versiones.csv` | 335,562 rows with the versions of each model, price, emissions and engine size |
| `Ventas_nuevas.json` | New car sales per model and year |
| `Precio_historico.xml` | Historical prices |
| `Consumos_emisiones.csv` | Fuel consumption and emissions |
| `Modelos.json` | Car models |
| `Tipo_de_cambio.csv` | Currency exchange rates |
| `Tablas y Variables v2.pdf` | Description of the tables and variables |

## What we did

### 1. Data cleaning

The raw ads had many problems, so most of the work went here:

- Split the ad date into **month** and **year** columns using regex.
- Removed units and symbols from numeric columns (`mph`, `mpg`, `L`, `*`) and converted them to numbers.
- Replaced invalid values (`?`, text in numeric fields) with nulls.
- Filled nulls with the **mean** for numeric columns and the **most frequent value** for categorical ones.
- Exported the clean data to CSV for the Power BI dashboard.
- Encoded categorical columns with `LabelEncoder` for the models.

### 2. Exploration

Quick look at the data with pandas, seaborn and matplotlib, for example how top speed relates to the second-hand price.

### 3. Models

We tried three regression models with scikit-learn:

| Model | R² train | R² test | Result |
|---|---|---|---|
| **Random Forest** (10 trees) | 0.85 | 0.50 | Final model (test RMSE ≈ 22,000 USD) |
| Decision Tree | 0.99 | lower | Overfitting |
| Multiple Linear Regression | low | low | Underfitting |

### 4. Submission

The final model predicts the price of the 2021 ads (14,864 cars) and saves them in the format required by the organizers (`adv_id`, `price`).

## What I would improve

Looking back at it with more time:

- Keep the 2021 ads (the ones to predict) out of the training data. In the notebook the filter is applied but then overwritten, so they end up in the training set.
- Try gradient boosting models like **XGBoost** or **LightGBM**, which were on our list but we had no time for.
- Use **cross-validation** and **hyperparameter tuning** instead of a single train/test split.
- Predict the logarithm of the price, since prices are very skewed.
- Use one-hot or target encoding instead of `LabelEncoder` for columns like maker and model.

## Repository structure

```
├── dataset.zip                  # Data provided by the organizers
├── notebooks/
│   ├── GAMMA - Modelo de prediccion.ipynb         # Cleaning, models and submission
│   └── GAMMA - Relacionando datos Ventas.ipynb    # Exploration: joining ads with sales data (work in progress)
├── final presentation/
│   ├── TEAM 20 (GAMMA).pdf      # Slides of the final pitch
│   └── TEAM 20 (GAMMA).pptx
└── trucos hackaton.txt          # Notes taken during the hackathon
```

The notebooks and notes are in Spanish.

## How to run it

```bash
git clone https://github.com/gurzelai/hackathon-mercedes-benz-mad.git
cd hackathon-mercedes-benz-mad
unzip dataset.zip -d notebooks/dataset
pip install pandas numpy scikit-learn seaborn matplotlib jupyter
jupyter notebook notebooks/
```

The notebooks were written on Windows and read the data with paths like `dataset\Anuncios_usados.csv`. On macOS or Linux, change `\` to `/` in those paths.
