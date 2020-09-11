## Early stopping

- In a Sklearn cross validation framework (RandomizedSearchCV or GridSearchCV), in terms of XGBClassifier, there is no way to use the validation set (of each fold) as the early stopping eval set. A fixed dataset need to be used as the early stopping eval set, and does not change when training and validating each fold.
    - https://stackoverflow.com/questions/42993550/gridsearchcv-xgboost-early-stopping
    - https://stackoverflow.com/questions/43866284/grid-search-and-early-stopping-using-cross-validation-with-xgboost-in-scikit-lea

- n_estimators (which is the number of boosting rounds) is a scikit-learn version of original version's num_boost_round parameter.
    - https://github.com/dmlc/xgboost/issues/4909
    - https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/

- Instead of finding the best early_stopping_rounds, I think finding the best n_estimators (i.e. num_boost_round) by trying a lot of values is an alternative way to find "how deep to go". Of course, early stopping won't happen and total training time will be longer.
    - https://blog.cambridgespark.com/hyperparameter-tuning-in-xgboost-4ff9100a3b2f
    - https://datascience.stackexchange.com/questions/58225/xgboost-rounds-is-equal-to-n-estimators
    - Implied by https://discuss.analyticsvidhya.com/t/optimize-n-estimators-using-xgb-cv/67850

Other ways:
    - A wrapper around XGBClassifier should enable using the CV validation set as the early stopping eval set.
    - DIY a cross validation framework.
    - Use the native learning API's xgboost.cv function.