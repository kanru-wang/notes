## Cohen's Kappa

From:

- http://kagglesolutions.com/r/evaluation-metrics--quadratic-weighted-kappa
- https://www.kaggle.com/aroraaman/quadratic-kappa-metric-explained-in-5-simple-steps

The Kappa coefficient is a chance-adjusted index of agreement. In machine learning it can be used to quantify the amount of agreement between an algorithm's predictions and some trusted labels of the same objects. Kappa starts with accuracy - the proportion of all objects that both the algorithm and the trusted labels assigned to the same category or class. However, it then attempts to adjust for the probability of the algorithm and trusted labels assigning items to the same category "by chance." This metric typically varies from 0 (random agreement between raters) to 1 (complete agreement between raters). In the event that there is less agreement between the raters than expected by chance, the metric may go below 0.

Notice that in terms of weighted matrix, "predictions that are further away from actuals are marked harshly than predictions that are closer to actuals".

## F beta

From: 

- https://www.kaggle.com/mnpinto/imet-fastai-starter
- https://www.kaggle.com/kenmatsu4/f2-score-for-keras
- https://scikit-learn.org/stable/modules/generated/sklearn.metrics.fbeta_score.html

F_2 weighs recall higher than precision, and F_0.5 weighs recall lower than precision.

When using NN for multi-label classification tasks, there is a best threshold that can be found by trying all thresholds and see which threshold generates the best F_2 value. Finding the best thresold is part of the task of a competition that uses F beta as the evaluation metric.

F-beta score can be calculated for the positive class in binary classification. For the multiclass task, it is the average (or weighted average) of the F-beta score of each class. The definition of "average" has to do with "micro" and "macro".

"Weighted average", which is an option in Sklearn's `f1_score`, can alter "macro" to account for label imbalance.

## Label ranking average precision

https://www.geeksforgeeks.org/multilabel-ranking-metrics-label-ranking-average-precision-ml/

## Customize Sklearn cross validation evaluation metric

https://scikit-learn.org/stable/modules/generated/sklearn.metrics.make_scorer.html
