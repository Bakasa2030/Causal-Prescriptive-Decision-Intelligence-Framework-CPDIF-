# -*- coding: utf-8 -*-
"""
CPDIF: COMPLETE END-TO-END EXPERIMENT
Dataset: Hugging Face criteo/criteo-uplift

This single script:
1. Downloads and samples the Criteo uplift dataset.
2. Creates one reproducible stratified train/validation/test split.
3. Benchmarks 12 uplift estimators and uses validation-selected CPDIF-S-LR as the single primary model.
4. Benchmarks 12 causal-uplift estimator configurations.
5. Estimates regression-adjusted and propensity-score-matched ATEs.
6. Runs covariate-dependent treated-arm subsampling experiments.
7. Produces counterfactual ITEs, response segments and targeting policies.
8. Evaluates policies using randomized-trial IPW estimates and the specified
   economic settings.
9. Runs threshold sensitivity and paired bootstrap uncertainty analyses.
10. Produces figures, CSV tables, LaTeX tables, fitted models and metadata.

IMPORTANT EXPERIMENTAL IMPLEMENTATION NOTES
- CPDIF-S-LR (S-learner with logistic regression) is the single primary CPDIF ITE and policy model.
- CPDIF-S (XGBoost) and the other configurations are benchmark comparators only.
- The additional learners are estimator comparisons within the ITE stage;
  they do not replace the CPDIF framework.
- SHAP explains the predictive outcome model associationally; it is not a
  causal explanation.
- The randomized assignment propensity is set to 0.85 in this configuration.
  Verify this against the dataset's experimental documentation before
  interpreting IPW/AIPW estimates.
- `visit` and `exposure` are excluded from baseline predictors.
- All model selection is based on validation Qini AUC. Test outcomes are
  reserved for final evaluation.
- The controlled-confounding experiment selectively retains treated records
  using the specified logistic covariate-dependent selection mechanism.
- Monetary ROI is evaluated using USD 50 per incremental conversion and
  USD 0.10 advertising cost per targeted user.
"""
