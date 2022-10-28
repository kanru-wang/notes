### Linear Regression Remedial Measures

- Tests
    - Normality of the residuals, Homoscedasticity, and Influence tests. See https://www.statsmodels.org/dev/examples/notebooks/generated/regression_diagnostics.html
    - Variance Inflation Factor for Multicollinearity
    - Durbin Watson for Autocorrelation
- Remedial Measures
    - Nonlinear relationships: transformations on X, e.g. X ** 2, log(X); or transformations on Y, e.g. log(Y)
    - Nonnormal Errors or non-constant error variance: transformations on Y, e.g. log(Y)
    - May use Box-Cox to suggest a transformation on Y.
    - https://www.researchgate.net/publication/308792602_REGRESSION_DIAGNOSTICS_AND_REMEDIAL_MEASURES
        - If x1, x2 and x3 demonstrate multicollinearity, we may combine them, e.g. x = (x1+x2)/x3 or x = x1x2x3, or use L1 / L2
    - https://www.stat.purdue.edu/~boli/stat512/lectures/topic2.pdf
        - 1. If the residuals appear to be normal with constant variance, and the relationship is linear, then go ahead with the regression model.
        - 2. If the residuals appear to be normal with constant variance, but the relationship is non-linear, try transforming the X’s to make it a straight line.
        - 3. If the residuals are badly-behaved, try transforming Y . If that stabilizes the variance but wrecks the straight line, try transforming X as well to get the straight line back.
        - 4. Transformations might simultaneously fix problems with residuals and linearity (and normality).
        - Remember that if you choose a transformation, you need to go back and do all the diagnostics all over again.