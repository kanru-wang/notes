# customize_loss_function

## Regression

- https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d
- https://github.com/manifoldai/mf-eng-public/blob/master/notebooks/custom_loss_lightgbm.ipynb

## Classification

Chat with Ryan Wu 06/22/2020

- Customized loss function is necessary to handle cases where FN is much worse than FP, etc.
- Use customized loss function for both training and CV.
- Do it by `scale_pos_weight` in xgboost, and also tune this hyper-parameter. (Perhaps use it for imbalanced data, and stop oversampling.)
- May need to factor in any class imbalance when designing a loss function.
- If the implementation is to pick top 100 customers to apply treatment, customized loss function is not useful, just use AUC.
- If the implementation is to decide a threshold, customized loss function is useful.
- Not every evaluation metric has its corresponding loss function. E.g. in regression, MSE has its corresponding loss function, but MAE doesn't (because it's not differentiable).
