## Classification model feature stability analysis

- Feature stability analysis allows us to avoid the following situation (Concept Drift): For example, there is a feature representing how many times a customer has used a certain service so far. In the training data, the frequent value range of this feature may be between 0 and 5. Months after the model deployment, the value range may become 0 to 15. A value of 5 no longer represents frequent usage of that service.

### When the class mixture is stable

        df.groupby("month").pivot_table(index="month", columns="class_label", values=list_of_features_for_investigation, aggfunc="mean")
        df["ratio_true_false_mean"] = df["positive"] / df["negative"]

If the "ratio_true_false_mean" for a long period always > 1 and for another long period always < 1, then this feature will likely to mislead the model.

### When the class mixture is unstable

- For many projects, in the training data the class mixture is unstable over time (due to data source changes, population mix changes), so for each feature its PSI is certainly unstable.
- To set it properly, we can first sample (with replacement) according to a fixed class proportion, and then do PSI analysis. The process is:

        set a class proportion, keep it fixed
        for each month
            sample according to the fixed class proportion
                do PSI analysis

Implementation:

- Create a column represents the weighting:

        Outside of the monthly for-loop:
        overall_class_proportion = y_train.value_counts('pct') # actually can be any weights

        Inside of the monthly for-loop:
        monthly_class_proportion = y_monthly.value_counts('pct')
        monthly_weight_dict = (overall_class_proportion / monthly_class_proportion).to_dict()
        X_monthly["weight"] = sklearn.utils.compute_sample_weight(
            class_weight=monthly_weight_dict,
            y=y_monthly
        )

- pd.DataFrame.sample
