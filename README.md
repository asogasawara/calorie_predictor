# 🍎 Calorie Predictor

Predicting caloric content of foods from macronutrient composition using regression models.

> DS3000 Final Project — Felix Watt, Aidan Ogasawara, Edison Chen

## Overview

This project explores whether calories can be reliably predicted from a food's macronutrient profile (protein, fat, and carbohydrates). We pull nutritional data from the **USDA FoodData Central API**, clean and process it, then train and evaluate multiple regression models.

## Data

- **Source:** [USDA FoodData Central API](https://api.nal.usda.gov/fdc/v1/foods/search)
- **Foods sampled:** Chicken, salmon, broccoli, rice, beef, pork, lamb
- **Features:** Protein (g), fat (g), carbs (g), sugars (g), fiber (g), sodium, cholesterol, saturated fats, added sugars
- **Preprocessing:** Column standardization, NaN removal, IQR-based outlier filtering

## Models & Results

| Model | Task | R² | MSE | Avg Error |
|-------|------|----|-----|-----------|
| **Linear Regression** | Calories from protein, fat, carbs | 0.437 | 2996.54 | ~54.7 cal |
| **Polynomial Regression** | Calories (with interaction/squared terms) | 0.454 | 2906 | ~53.9 cal |
| **Linear Regression** | Protein from cholesterol (red meats) | 0.521 | 23.7 | ~4.9 g |

**Key finding:** Polynomial regression offered negligible improvement over linear, suggesting the calorie–macronutrient relationship is fundamentally linear. The third model showed cholesterol is a reasonable predictor of protein content, though accuracy decreases at higher cholesterol levels (heteroscedasticity).

## Pipeline

```
USDA API → Data Cleaning → EDA & Visualization → Model Training → Residual Diagnostics
```

1. **`get_macros(food)`** — Queries the USDA API (50 items per call)
2. **`clean_macros()`** — Standardizes columns, selects features, converts types, drops NaNs
3. **`remove_outliers_iqr()`** — Filters extreme values via interquartile range
4. Visualization with Seaborn & Matplotlib
5. Model fitting & residual analysis (independence, homoscedasticity, normality)

## Tech Stack

- Python
- Pandas / NumPy
- Scikit-learn
- Seaborn / Matplotlib
- USDA FoodData Central API

## Future Directions

- Add alcohol content and fiber as predictors
- Engineer features for food processing level (minimal → high)
- Analyze prediction errors by food category (fried, fermented, etc.)
- Apply PCA to identify the most impactful predictors
