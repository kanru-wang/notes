# Area under Precision-Recall Curve vs AUROC

- Area under Precision-Recall Curve is better than AUROC when class is highly imbalanced.
- AUROC is generated from TP rate (TP / (TP + FN)) and FP rate (FP / (FP + TN)).
- AUPRC is generated from Recall (i.e. TP rate) and Precision (TP / (TP + FP)).

From: https://machinelearningmastery.com/roc-curves-and-precision-recall-curves-for-classification-in-python/

- AUROC is being optimistic (or in my words, skewed), which is mainly because of the use of **True Negatives in the False Positive Rate** in the ROC Curve. In the Precision-Recall curve, True Negative is not used in the calculation.
- There is an advantadge of AUROC which AURPC does not have:

        If the proportion of positive to negative instances changes in a test set, the ROC curves will not change. 
        Metrics such as accuracy, precision, lift and F scores use values from both columns of the confusion matrix. 
        As a class distribution changes these measures will change as well, 
        even if the fundamental classifier performance does not. 
        ROC graphs are based upon TP rate and FP rate, in which each dimension is a strict columnar ratio, 
        so do not depend on class distributions.

From: https://towardsdatascience.com/why-you-should-stop-using-the-roc-curve-a46a9adc728

- The value of ROC-Area increases linearly as the quality of the model improves, whereas Average-Precision grows exponentially.
- The behavior of Average-Precision is more expressive in getting a flavour of how the model is doing, because it is more sensible in distinguishing between a good and a very good model. Moreover, it is directly linked to precision: an indicator which is human-understandable.