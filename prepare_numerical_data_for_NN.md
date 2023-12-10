## Prepare Numerical Data for Neural Networks

- Before applying neural networks to tabular data, we need to pre-process the tabular data
- The goal is to ensure input features for a neural network
    - To be between 0 and 1
    - If
      - Regression or Classification, then missing values filled with mean or median
      - Autoencoder, then missing values filled with values that are separate enough from non-missing values
    - Non-missing values should spread in a wide range (between 0 and 1), instead of all “congested” in a small range.
- Do the following steps for every feature column:
  	1. Clip extreme values (both high and low ends) according to the scale of values (in the feature)
  	2. If
  	    1. Regression or Classification, then fill NA with mean or median of the feature
  	    2. Autoencoder, then fill NA with a negative value according to the scale of values (in the feature). E.g. the filling value can be `percentile_1 – 1 – (percentile_99 – percentile_1)`
  	4. Try to shift all values to positive (so we can take log)
  	5. Take the log of all values to remove any long positive tail
  	6. Further fill missing values with 0s to handle log(negative_value)
  	7. Scale all values to between 0 and 1
- Also consider Winsorizing, or discretizing a feature into for example 100 bins.
