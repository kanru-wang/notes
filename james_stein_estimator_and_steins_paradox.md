### James Stein Estimator and Stein's Paradox

From: https://www.youtube.com/watch?v=cUqoHQDinCM

- When three or more parameters (even if the parameters are independent) are estimated simultaneously, shrinking the estimators using the James Stein Estimator will result in more accuracy on average (i.e. having lower expected mean squared error) than estimating each parameters separately.
- I think the intuition is that, in the video's context, it is more frequent to get a sample that is outside of the ball (far end) than inside of the ball (near end).
- Shrinking the weights (i.e. adding some small Bias) can result in higher Variance but lower Mean Squared Error.
