# A Global Investigation of Female Labour Force Participation and its Determinants

Panel data analysis testing whether cross-country associations between political empowerment and female labour force participation (FLFP) hold up once country-level heterogeneity is controlled for.

## Data
Seven datasets merged from Our World in Data (FLFP, fertility, age dependency ratio, gender gap in tertiary education, political empowerment index, GDP per capita), covering **42 countries from 1990–2020** (1,007 country-year observations after cleaning).

## Method
- **Multiple imputation** (`mice`, m = 28, PMM) to handle missingness (up to 27.5% for gender gap), following White, Royston & Wood's (2010) rule that m should be at least the percentage of incomplete cases. MAR assumption tested via a missingness-indicator regression.
- **Variable selection**: backward/forward stepwise AIC, LASSO, Ridge, and Random Forest compared across all 28 imputations to check stability of predictor inclusion.
- **Two-way fixed effects panel regression** (`plm`, within estimator) with **Driscoll-Kraay standard errors** (`vcovSCC`) to correct for cross-sectional dependence and serial correlation, pooled across imputations using Rubin's rules via a custom `pool_dk()` function.
- **Hausman test** to formally justify fixed effects over random effects.
- **Mundlak within-between decomposition** (`lmer`) to separate each predictor's within-country and between-country effects.
- **Robustness checks**: outlier exclusion, listwise deletion, one-year lagged specification, multicollinearity diagnostics (VIF).

## Key Results
- Pooled OLS shows a large, highly significant effect of political empowerment on FLFP (β = 25.10, p < 0.001) — but this **collapses to null** once two-way fixed effects are introduced (β = 4.31, p = 0.485), validating Gaddis and Klasen's (2013) critique of cross-sectional analysis.
- Hausman test confirms fixed effects over random effects (χ² = 17.91, p = 0.003).
- Mundlak decomposition reveals political empowerment's within-country and between-country coefficients differ in **both magnitude and direction** (1.67 vs. −18.01), showing pooled OLS conflates structural cross-country differences with within-country dynamics.
- Fertility is the most robust within-country predictor (β = 3.08, p = 0.043 once collinearity with age dependency is addressed).
- Results are robust to outlier exclusion, listwise deletion, and lagged specifications.

## Tools
R: `plm`, `mice`, `lme4`, `sandwich`, `glmnet`, `randomForest`, `ggplot2`, `car`.
