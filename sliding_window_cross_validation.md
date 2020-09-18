# Sliding window cross validation

From: https://towardsdatascience.com/introducing-walk-backward-a-novel-approach-to-predicting-the-future-c7cf9e15e9e2

Think about cases where you observe customer transactions in the last three months and predict the next one month. The "Performance Period" is one month where you get the target labels, and the "Observation Period" is a three-month period where you get the features. This problem is different from the problem stated in tabular_data_train_val_split.md, because the problem here requies an observation period (instead of being able to get immediate outcome).

    * Observation period
    ^ Performance period
                                      now
    window 1           ************^^^^|
    window 2       ************^^^^    |
    window 3    ************^^^^       |
    final model        ************^^^^|
    application            ************|????
        
From my experience, we need to:

- Avoid having rows from a same customer but different windows used in both training and validation sets (Data leakage)
- Avoid having rows from different customers but a same window used in both training and validation sets (Data leakage, but less severe. This problem is due to trends, e.g. market conditions, marketing campaigns, etc.)

We certainly want to avoid the first point. If we also want to avoid the second point, we need to do grouped k-fold cross validation twice to construct train/val datasets that meet both requirements.

- If we have 3 windows, we can do
    - train (window 1 and 2), val (window 3)
    - train (window 1 and 3), val (window 2)
    - train (window 2 and 3), val (window 1)
- Assume we have 6 customers, a, b, c, d, e, f
- Now we can have (window, customer) combinations shown in below
- Apply sklearn grouped m-fold cross validation on window (in our case m = 3), apply grouped n-fold cross validation on customer ID (say n = 3, but can also be 1, 2 or 6 in our case). This will result in a cross validation of m * n = 3 * 3 = 9 folds.
- For each fold, we have a view shown in below. If both assignments are "train", that sample will be "train". If both "test", then "test". If conflict, then discard.

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

I even think since "window 2" is similar to "window 1" and "window 3", it should be removed. So unless we have a long period of historical data that allows multiple non-overlapping windows, we should just have two windows that are as far apart as possible. There can be a cross validation of m * n = 2 * 4 = 8 folds. Here n = 3, 4 or 5 so that the training data size will be 66%, 75%, or 80% of the final model data size.

    * Observation period
    ^ Performance period
                                      now
    window 1           ************^^^^|
    window 2    ************^^^^       |
    final model        ************^^^^|
    application            ************|????
    
The other option, as the author of the quoted article said, if window 1 has enough data, no need to cross validate by training on window 2 and validate on window 1. https://medium.com/@kevinwang0811/nice-article-8f44100411c4