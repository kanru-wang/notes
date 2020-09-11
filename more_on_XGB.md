## Early stopping

- In a sklearn cross validation framework, there is no way to pass the validation set as early stopping eval set to XGBClassifier.
    - https://stackoverflow.com/questions/42993550/gridsearchcv-xgboost-early-stopping
    - https://stackoverflow.com/questions/43866284/grid-search-and-early-stopping-using-cross-validation-with-xgboost-in-scikit-lea

- n_estimators (which is the number of boosting rounds) is a scikit-learn version of original version's num_boost_round parameter.
    - https://github.com/dmlc/xgboost/issues/4909
    - https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/
        
- I think finding the best n_estimators is an alternative to finding the best early_stopping_rounds.
    - https://datascience.stackexchange.com/questions/58225/xgboost-rounds-is-equal-to-n-estimators
    - Implied by https://discuss.analyticsvidhya.com/t/optimize-n-estimators-using-xgb-cv/67850

- I think a wrapper around XGBClassifier should enable using the CV validation set as the early stopping eval set.