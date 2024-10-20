## How to Monitor Model Performance and Treatment Effectiveness

Problem description:

  1. A successful retention/cross-sell model would detect customers likely to churn/buy.
  2. These customers would be treated; some of them would therefore stay/buy.
  3. Both the **model performance** and the **retention/cross-selling efforts** would be reflected in the ground truth labels.
  4. The Precision would be worse/better than what would be observed if retention/promotion doesn’t exist.

A solution is to, for each percentile bin, reserve the same number of customers not to be treated. Specifically:
- We have 3 groups of customers:
  - **Group 1**: high propensity to churn/buy and treated; use full data
  - **Group 2**: high propensity to churn/buy but not treated; this is the control group
  - **Group 3**: low propensity and not treated; it is sampled
  - (High propensity customers are those who are eligible for the treatment)
- Among high propensity customers (Group 1 vs 2), comparing the positive rate of the “treated” vs “not treated” tells us how effective the **retention/cross-sell efforts** are at each percentile.
- Among not-treated customers (Group 2 + 3), ensure each percentile bin has the same sample size, then calculate the **Precision** and so on.

--------

Furthermore, there is another problem that can be described as:

  1. After the model goes in production, untreated customers (in the total customer pool) will gradually be exhausted.
  2. After the first month, we will have no option but to treat customers most of whom will not have highest propensities (because those with highest propensities are already treated in earlier months).
  3. Therefore, every month the **retention/cross-selling effort effectiveness** will usually seem worse than the previous month.

A solution is to measure the retention/promotion effort effectiveness on ground truth labels not within every month, but over a moving windows of n month, where n is, for treated customers, how many months we wait until it is proper and worth to treat them again.

The **model performance** can still be accurately measured every month. Every month a new Group 2 is formed (which contains last month’s Group 2 customers that still exist and remain high propensity + some customers newly become high propensity). Every month’s Group 2 should strive for overlapping customers with the last month’s Group 2 as many as possible to avoid control group shrinkage.
