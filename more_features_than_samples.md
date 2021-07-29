## The problem of having more features than minority class sample size

https://stats.stackexchange.com/questions/498797/logistic-regression-ignore-one-in-ten-rule-if-regularization-is-implemented
 
Points to:
 
`Use LASSO, ridge regression, or elastic net, which impose a penalty on regression coefficients that guards against overfitting… These methods provide principled ways to build models even if you have more predictor variables than cases.`
 
Also:
 
`Note that you can have "20 records and 60 predictors" or even more dramatic excesses of predictors to cases in some areas of interest, which can be called the "p≫n" problem. For example, studies of gene expression in biology can have almost 20,000 potential predictors… to associate with only a few dozen events (e.g., cancer deaths). Minimizing overfitting in such cases often involves a regularization approach like LASSO or ridge regression, which puts a penalty on the magnitudes of regression coefficients (many or most penalized down to 0 in LASSO)… These methods can be thought of as reducing the effective numbers of predictors as their coefficient magnitudes are reduced.`