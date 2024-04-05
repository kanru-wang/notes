## Survival Analysis

<img src="image/survival_analysis_functions.png" width="800"/>

Survival Analysis Benefits
- https://medium.com/dataminingapps-articles/using-survival-analysis-to-model-time-to-default-f918e6d5ff36

Censoring and Truncation
- https://youtu.be/K-_sblQZ5rE

Hazard and Survival Functions
- https://youtu.be/zAdF8WSyfsA

Cox Proportional Hazards Model (Semi-Parametric Model, models the hazard/risk)
- https://medium.com/utility-machine-learning/survival-analysis-part-1-the-weibull-model-5c2552c4356f
- https://medium.com/utility-machine-learning/survival-analysis-part-2-taking-advantage-of-static-data-28acd09fa2d4
- First half of: https://youtu.be/W9_V-TTOCPM and https://youtu.be/JUaZK9TchCU
  - A positive coefficient of a variable means that the higher the variable the higher the risk 

Accelerated Failure Time (Parametric Model, models the time-to-event)
- https://xgboost.readthedocs.io/en/stable/tutorials/aft_survival_analysis.html
  - Express the labels in the form of a range, so that every data point has two numbers associated with it, namely the lower and upper bounds for the label. For uncensored labels, use a degenerate interval of form `[a, a]`.
- First half of: https://youtu.be/Mfq8vWOGTQo and https://youtu.be/JUaZK9TchCU
  - A positive coefficient of a variable means that the higher the variable the lower the risk.
- https://lifelines.readthedocs.io/en/latest/Survival%20Regression.html#accelerated-failure-time-models
  - Unlike XGBoost, no need to specify the lower and upper bounds for the label, due to its input data format different from XGBoost's.
- First half of: https://myweb.uiowa.edu/pbreheny/7210/f15/notes/10-15.pdf
  - Whereas in a proportional hazards (PH) model, the covariates act multiplicatively on the hazard, in an AFT model the covariates act multiplicatively on time.

Construct a dataset in one of the following ways
- Duration is measured from when each customer starts to exist, until its event (or censor) date. We would not have window features.
- Given source data is available from time `t` and window features take a span of `s`, we model all customers that already have a history of `s` at the time `t + s`. We would not have window features. In production, we would not be able to apply this model to relatively new customers who have a tenure <= `s`.
- Starting date is a randomly chosen month between 1 to n before the event (or censor) date. Probability mass function of event customers is used to randomly generate censor dates for non-event customers, so that (1) observation periods for event and non-event samples are consistent, (2) the test set can be naturally created. Furthermore, window features are available for modeling.

Prioritize contacting customers whose predicted survival falls below a certain threshold.

Additionally, customer lifetime value can be calculated by combining expected survival times with monthly revenues.
