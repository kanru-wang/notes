### Classification model feature stability analysis

- Feature stability analysis allows us to avoid the following situation: For example, there is a feature representing how many times a customer has used a certain service so far. In the training data, the frequent value range of this feature may be between 0 and 5. Months after the model deployment, the value range may become 0 to 15. A value of 5 no longer represents frequent usage of that service.
- For many projects, in the training data the class mixture is unstable over time (due to data source changes, population mix changes), so for each feature its PSI is certainly unstable. To set it properly, PSI analysis should be **broken down by classes (target variable)** (i.e. PSI analysis for each feature for each class). However, it has the following disadvantages:
    - For a certain feature, all classes' PSI values may be unstable, but this feature may still be useful for the model. For example:
        - For a certain feature, for each class, the feature values have high variance, but the model is still able to use this feature to seperate classes.
        - For a certain feature, for each class, the feature values are trending down, but the model is still able to use this feature to seperate classes.
    - For a certain feature, some classes' PSI values are unstable, while others are stable. Should we remove or keep this feature?
- The solution is a "feature usefulness stability analysis". Usefulness is what really matters.
    - In theory, this can be done by training a model on each month's data, and look at the feature importance of each month's model.
        - If the feature importance of a feature is trending down, or if it is very unstable, we should remove this feature.
        - Don't remove a feature if its feature importance is trending up, or a bit unstable.
    - In practice, because training a model for each month's data is too slow, we can use a fast univariate feature selection method to determine how useful a feature is at a given month.
        - For a given feature, we apply this method on each month's data, and then we get a feature usefulness trend.
        - Possible methods are `sklearn.feature_selection.SelectKBest()` and `phik` package's `phik_matrix()`.
        - Both methods work for multi-class feature selection. Regardless of how many classes are there, both methods can give a single clear number to represent how useful a feature is at helping the model to seperate classes, which is helpful for automation.