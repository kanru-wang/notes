## Cohen's Kappa

From:

- http://kagglesolutions.com/r/evaluation-metrics--quadratic-weighted-kappa
- https://www.kaggle.com/aroraaman/quadratic-kappa-metric-explained-in-5-simple-steps

The Kappa coefficient is a chance-adjusted index of agreement. In machine learning it can be used to quantify the amount of agreement between an algorithm's predictions and some trusted labels of the same objects. Kappa starts with accuracy - the proportion of all objects that both the algorithm and the trusted labels assigned to the same category or class. However, it then attempts to adjust for the probability of the algorithm and trusted labels assigning items to the same category "by chance." This metric typically varies from 0 (random agreement between raters) to 1 (complete agreement between raters). In the event that there is less agreement between the raters than expected by chance, the metric may go below 0.

Notice that in terms of weighted matrix, "predictions that are further away from actuals are marked harshly than predictions that are closer to actuals".