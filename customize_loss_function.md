# customize_loss_function

## Regression

- https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d
- https://github.com/manifoldai/mf-eng-public/blob/master/notebooks/custom_loss_lightgbm.ipynb

- https://heartbeat.fritz.ai/5-regression-loss-functions-all-machine-learners-should-know-4fb140e9d4b0

    - One problem in using MAE loss is that its gradient is the same throughout, which means the gradient will be large even for small loss values. This isn’t good for learning. To fix this, we can use dynamic learning rate which decreases as we move closer to the minima. MSE behaves nicely in this case and will converge even with a fixed learning rate. The gradient of MSE loss is high for larger loss values and decreases as loss approaches 0, making it more precise at the end of training (see figure below.) MAE loss is more robust to outliers, but its derivatives are not continuous, making it inefficient to find the solution. MSE loss is sensitive to outliers, but gives a more stable and closed form solution (by setting its derivative to 0.)

    <img src="image/mae_mse.png" width="700"/>
    
    - Huber Loss combines good properties from both MSE and MAE, but need to define the hyperparameter delta.
    

## Classification

Chat with Ryan Wu 06/22/2020

- Customized loss function is necessary to handle cases where FN is much worse than FP, etc.
- Use customized loss function for both training and CV.
- Do it by `scale_pos_weight` in xgboost, and also tune this hyper-parameter. (Perhaps use it for imbalanced data, and stop oversampling.)
- May need to factor in any class imbalance when designing a loss function.
- If the implementation is to pick top 100 customers to apply treatment, customized loss function is not useful, just use AUC.
- If the implementation is to decide a threshold, customized loss function is useful.
- Not every evaluation metric has its corresponding loss function. E.g. in regression, MSE has its corresponding loss function, but MAE doesn't (because it's not differentiable).
