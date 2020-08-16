# Credit Risk Feature Creation Digust

Source: 
1. https://mp.weixin.qq.com/s/FX_CDnd8OBcvlwIpWNDVPw
2. https://mp.weixin.qq.com/s/MG41BuPFC0aIh7M1y_u4dA

- Features based on the idea of RFM (Recency, Frequency, Monetary) are useful.
- Be aware that features like total_spending_last_12_mth do not make sense to a new customer joined for just 6 months.
- Benefit of using ratio, e.g. pct_fine_dining_spending = total_fine_dining_spending / total_spending
    - Is it a treat, or part of a luxry lifestyle?
    - Scale normalized (although not useful for tree based models)
- Benefit of using slope, e.g. (this_mth_n_debit - last_mth_n_debit) / last_mth_n_debit
    - Represents trend
- Benefit of using "coefficient of variation" which is (standard deviation / mean) * 100%
    - Represents stability
    - A customer stably makes lots of credit card transactions is safer than doing that unstably
- The article mentiones using PSI and IV (Information Value) to only include stable and useful features in the model.
- The article mentiones Observation Window and Performance Window. From my experience, if we want to predict 4-6 months in the future, we not only need a 4-month gap where we cannot use for generating features, but need to exclude customers who have positive events happen within the 4-month gap as well.