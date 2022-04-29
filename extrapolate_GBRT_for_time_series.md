### Use Gradient Boosted Regression Tree / XGBoost for time series, how to extrapolate?

- First build a linear regression that can model the trend pattern. The predicted trend is subtracted from the training data (to make them stationary) and then added again at prediction time. From: https://towardsdatascience.com/is-gradient-boosting-good-as-prophet-for-time-series-forecasting-3dcbfd03775e

- (1) Combine XGBoost with another model that can extrapolate, or (2) Remove non-stationary effects. From: https://towardsdatascience.com/xgboost-for-time-series-youre-gonna-need-a-bigger-boat-9d329efa6814

- Differencing the time series to make it stationary, and later transform the predictions back by computing a cumulative sum from previous values. From: https://shanminlin.medium.com/how-to-help-tree-based-models-extrapolate-7954287b1219

- lightGBM can extrapolate. From: https://towardsdatascience.com/xgboost-for-timeseries-lightgbm-is-a-bigger-boat-197864013e88