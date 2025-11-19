# JAMB Score Prediction and Student Performance Insights

Predict JAMB exam scores and provide students with actionable performance expectations and trend-based recommendations. This project uses student features (for example: `Study_Hours_Per_Week`, `Parent_Education_Level`, `Socioeconomic_Status`, `Attendance_Rate`, `Teacher_Quality`, `Assignments_Completed`, `Age`, etc.) to model and explain expected performance, highlight strengths/weaknesses, and suggest study priorities.

## Motivation
- Purpose: Help students understand how study habits and background factors relate to expected JAMB performance, monitor trends, and prioritise improvement areas.
- Target users: Students preparing for JAMB, tutors, and education researchers.

## Dataset
- Source: `jamb_exam_results.csv` (included in this repo).
- Key columns:
	- `JAMB_Score` — target (numeric)
	- `Study_Hours_Per_Week` — hours studied weekly
	- `Parent_Education_Level` — categorical
	- `Socioconomic_Status` — categorical/ordinal
	- `Attendance_Rate`, `Teacher_Quality`, `Assignments_Completed`, `Age`, `Gender`, `School_Type`, etc.
- Notes: Missingness (for example in `Parent_Education_Level`) can be informative and should be handled carefully (impute or flag).

## Project Structure
- `jamb_exam_results.csv` — raw data
- `JAMB_score_pred.ipynb` — analysis, preprocessing, models, evaluation, visualizations
- `README.md` — this document

## Data Preparation
- Cleaning: handle missing values (impute, fill, or create missing indicators); remove identifiers like `Student_ID` before modeling.
- Encoding: label-encode binary fields; one-hot encode nominal categorical fields.
- Scaling: apply `StandardScaler` for distance-based models (KNN, MLP) when appropriate.
- Feature engineering: consider derived features such as `hours_per_assignment`, study intensity, or aggregated trend features where available.

## Modeling
- Problem type: Regression (predict continuous `JAMB_Score`). Optionally binarize into classes (e.g., pass/fail) for classification tasks and ROC/AUC analyses.
- Candidate models used in the notebook:
	- Linear Regression
	- Random Forest Regressor
	- K-Nearest Neighbors Regressor
	- MLPRegressor (neural net)
- Hyperparameter tuning: use `GridSearchCV` with a `models_grid` dictionary (see notebook) to evaluate and select models using cross-validated metrics such as `neg_mean_squared_error` or `r2`.

## Evaluation & Metrics
- Regression metrics: `R^2`, `RMSE`, `MAE`.
- If the target is binarized: precision, recall, ROC curve, AUC, and precision–recall curve (useful for imbalanced classes).
- Practical checks: cross-validate results (K-Fold), inspect residual plots, and verify calibration across subgroups (e.g., by `Socioeconomic_Status`).

## Feature Importance & Interpretability
- Per-feature AUC/ROC (for a binary target) helps identify discriminative features beyond Pearson correlation.
- Use permutation importance or SHAP values for model-agnostic explanations and to show which features most influence predictions.
- Focus on actionable features (e.g., study hours, assignments completed) when giving student advice; treat demographic features with care to avoid unfair recommendations.

## Student-Facing Outputs (UX)
- Input: student fills a form with current values (study hours, attendance, parent education, socioeconomic indicators).
- Output:
	- Predicted `JAMB_Score` with a confidence interval or expected score range.
	- Trend visualizations comparing the student's values to cohort percentiles.
	- Top 3 actionable recommendations (e.g., increase weekly study hours, complete more assignments) with estimated impact on predicted score.
	- Explanation of which features contributed most to the prediction (via SHAP/permutation importance).

## Quick Start (Notebook)
1. Install dependencies:

```powershell
pip install -r requirements.txt
```

2. Open the notebook:

```powershell
jupyter notebook MLZOOMCAMP_MIDTERM/JAMB_score_pred.ipynb
```

3. Notebook workflow:
	- Load `jamb_exam_results.csv` and inspect data
	- Clean and preprocess (impute or flag missing values, encode categoricals, scale numeric features)
	- Split data into train/test and run `GridSearchCV` over `models_grid`
	- Evaluate model(s), visualize ROC/AUC (if classification), and inspect residuals (regression)
	- Create a student-facing inference demo (single-row prediction + explanation)

## Reproducibility
- Set random seeds (e.g., `random_state=42`) for reproducibility.
- Record environment dependencies in `requirements.txt` and include Python version.

## Ethics & Limitations
- Bias: demographic features may reflect structural inequalities — be cautious when using these features for recommendations.
- Privacy: remove personally identifying information (e.g., `Student_ID`) before sharing or deploying models.
- Not a guarantee: model outputs are probabilistic estimates for guidance, not deterministic predictions of exam performance.

## Next Steps
- Build a small web demo (Streamlit or Flask) so students can input their data and get immediate feedback.
- Add SHAP visual explanations for single predictions.
- Expand features with subject-wise study breakdowns, historical performance, or time-series tracking for trend-based recommendations.

## Contributing
- To contribute: fork the repo, add improvements (models, visualizations, UI), and open a pull request. Include tests or example notebooks when possible.

---
