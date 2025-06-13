
# 🦟 Dengue Modelling Dashboard

This dashboard provides an interactive platform for modelling the delayed effects of weather on dengue cases using Ordinary Least Squares (OLS) and Poisson regression models. Users can upload their own datasets, select optional variables, and explore the best-fitting models along with diagnostics and visualisations.

The dashboard is available at: https://dengue-dashboard.onrender.com/ 

Developed by **Avik Kumar Sam**, under mentorship from Prof. Harish C. Phuleria (IITB) and Dr. Ipsita Pal Bhowmick (ICMR).  
**Reference**: [Jena et al., 2025](https://doi.org/10.1080/22221751.2025.2493924)

---

## How to Use

1. **Data Requirements**:
   - Data should be **monthly**, in **Excel format** (`.xls`/`.xlsx`).
   - Sheet name must be `Final`.
   - Required columns:
     - `Cases`: Monthly dengue cases
     - `Temperature`, `RH` (Relative Humidity), `Precipitation`
     - `T_YA`, `RH_YA`, `Prec_YA`: Yearly averages
     - `Seasons`: Categorical (values = "Winter", "Monsoon", "Post-monsoon")
     - `Year`, `Month`: Temporal identifiers

2. **Upload Instructions**:
   - Drag and drop or select your Excel file in the dashboard.
   - Select additional variables (yearly averages, seasons, year) if desired.
   - Choose model type: **Log-linear (OLS)** or **Poisson**.
  
3. **You may use the excel file template available here: https://github.com/sam-as/Dengue-Dashboard-Models/blob/main/Dummy_data.xlsx**
   - Use the download icon ⬇️ next to the Raw Button.
   - ![image](https://github.com/user-attachments/assets/0b40153a-58d7-4e22-9b2c-b13c3410f0f7)


---

## Model Structure

### Variables:

- All combinations of:
  - `Temperature_lag1`, `Temperature_lag2`, `Temperature_lag3`
  - `RH_lag1`, `RH_lag2`, `RH_lag3`
  - `Precipitation_lag1`, `Precipitation_lag2`, `Precipitation_lag3`

- Optional variables:
  - Yearly averages: `T_YA`, `RH_YA`, `Prec_YA` (For each year, calculate the averages and add the data in your Excel sheet for each month of a particular year)
  - Seasonal dummies: `Season_Winter`, `Season_Monsoon`, `Season_Postmonsoon` (These should have 0 and 1)
  - Year (numeric) 

### Mathematical Form:

- **Log-linear Model**:
  \[
  \log(\text{Cases} + 1) = \beta_0 + \sum \beta_i X_i + \varepsilon
  \]

- **Poisson Model**:
  \[
  \log(\mathbb{E}[\text{Cases}]) = \beta_0 + \sum \beta_i X_i
  \]

Where \( X_i \) includes lagged weather variables and optional Variables.

---

## Output & Interpretation

### Best Model Selection

- **OLS**: Based on highest **Adjusted R²**
- **Poisson**: Based on lowest **AIC**

### Summary Table

Each model combination is evaluated and ranked. The summary includes:

| Model Formula | Adjusted R² (OLS) | AIC | BIC |
|---------------|------------------|-----|-----|

**Interpretation**:
- **Adjusted R²**: Explains variance while penalising complexity. Higher is better.
- **AIC/BIC**: Penalize overfitting. Lower is better.

### Residual Analysis

- **Residuals vs Fitted Plot**:
  - Look for randomness around zero.
  - **Pattern or funnel shape?** → indicates **heteroscedasticity**.

- **Actual vs Predicted Plot**:
  - Ideally, the predicted curve follows the actual data.
  - Deviations highlight poor model fit or omitted variables.

---

## Coefficient Interpretation

Each variable is shown with its:
- Estimated coefficient
- Significance (denoted with `**` if \( p < 0.05 \))

Example:
```
log(Cases) ~ + 0.2312 x Temperature_lag1 ** - 0.0421 x RH_lag2 + ...
```

- **Positive coefficient**: Increase in variable → higher dengue risk.
- **Negative coefficient**: Increase in variable → lower dengue risk.
- **Statistical significance**: Only coefficients with `**` are statistically meaningful.
- **The interpretations for log-linear and Poisson are different.

---

## 🔬 LOOCV (Leave-One-Out Cross-Validation)

For each model, LOOCV is used to estimate the generalisation error. While the error is not directly reported in the dashboard, it helps stabilise model selection and avoid overfitting.

---


## 👤 Contact

**Avik Kumar Sam**  
📧 [avik.sam@iitb.ac.in](mailto:avik.sam@iitb.ac.in)  
🌐 [https://sites.google.com/view/aviksam](https://sites.google.com/view/aviksam)
