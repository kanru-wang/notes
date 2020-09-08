# Sliding window cross validation

From: https://towardsdatascience.com/introducing-walk-backward-a-novel-approach-to-predicting-the-future-c7cf9e15e9e2

Think about cases where you observe customer transactions in the last three months and predict the next one month. The "Performance Period" is one month where you get the target labels, and the "Observation Period" is a three-month period where you get the features.

    * Observation period
    ^ Performance period
                                   now
    window 1        ************^^^^|
    window 2    ************^^^^    |
    window 3 ************^^^^       |
    final model         ************|????
        
From my experience, we need to avoid:

- Rows from a same customer but different windows used in both training and validation sets (Data leakage)
- Rows from different customers but a same window used in both training and validation sets (Data leakage, but less severe. Due to trends, e.g. market conditions, marketing campaigns, etc.)

We certainly want to avoid the first point. If we also want to avoid the second point, we need to do grouped k-fold cross validation twice to construct train/val datasets that meet both requirements.

- If we have 3 windows, we can do
    - train (window 1 and 2), val (window 3)
    - train (window 1 and 3), val (window 2)
    - train (window 2 and 3), val (window 1)
- Assume we have 6 customers, a, b, c, d, e, f
- Now we can have (window, customer) combinations as such
- Apply sklearn grouped **3** fold cross validation twice (once on window and once on customer ID)
- For each of the 3 folds, we have a view as such. If both assignments are "train", that sample will be "train". If both "test", then "test". If conflict, then discard.

        (window, customer)  k-fold grouped on window  k-fold grouped on customer  overlapping
        1 a                 train                                          train        train
        2 a                 train                                          train        train
        3 a                 val                                            train        /
        1 b                 train                                          train        train
        2 b                 train                                          train        train
        3 b                 val                                            train        /
        1 c                 train                                          train        train
        2 c                 train                                          train        train
        3 c                 val                                            train        /
        1 d                 train                                          train        train
        2 d                 train                                          train        train
        3 d                 val                                            train        /
        1 e                 train                                          val          /
        2 e                 train                                          val          /
        3 e                 val                                            val          val
        1 f                 train                                          val          /
        2 f                 train                                          val          /
        3 f                 val                                            val          val
