## What causes high accuracy and low F1 at the same time?

Accuracy = (TP + TN) / (TP + TN + FP + FN)

F1 = 2 * (TP / (TP + FP)) * (TP / (TP + FN)) / ((TP / (TP + FP)) + (TP / (TP + FN)))

Therefore,

Accuracy / F1 = (TP * Accuracy + TP + TN - TN * Accuracy) / (2 * TP)

- What we care about is Accuracy / F1
- The higher TP, the lower Accuracy / F1
- The higher Accuracy, the higher Accuracy / F1, which is useless information.
- The higher TN, the higher Accuracy / F1