### Classification model feature stability analysis

- Feature stability analysis allows us to avoid the following situation: For example, there is a feature representing how many times a customer has used a certain service so far. In the training data, the frequent value range of this feature may be between 0 and 5. Months after the model deployment, the value range may become 0 to 15. A value of 5 no longer represents frequent usage of that service.
- For many projects, in the training data the class mixture is unstable over time (due to data source changes, population mix changes), so for each feature its PSI is certainly unstable.
- To set it properly, we can first sample (with replacement) according to a fixed class proportion, and then do PSI analysis. The process is:

        set a class proportion, keep it fixed
        for each month
            sample according to the fixed class proportion
                do PSI analysis