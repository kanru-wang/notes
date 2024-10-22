### Casual Inference

1. Difference in difference
2. Synthetic Control: https://youtu.be/1PQfeDT8zXM
3. Instrumental Variables: https://youtu.be/J2BMFBMO14o
4. Structural Equation Modeling: https://youtu.be/-m4ag3WQcCw

### Entropy

From: https://towardsdatascience.com/entropy-is-a-measure-of-uncertainty-e2c000301c2c

- Entropy can be used as a measure of uncertainty of multi-class classification
- The formula of entropy has its shape because it is the only family of functions that satisfies the four basic properties

From my experience

- Although [0.8, 0.2, 0, 0, 0] has a lower entropy than [0.8, 0.05, 0.05, 0.05, 0.05], sometimes the size of the gap between the highest and 2nd highest is more important than the entropy value.
- Sometimes caliberation is required before calculating the entropy.

### K Prototype Clustering

- https://antonsruberts.github.io/kproto-audience/
    - It measures distance between numerical features using Euclidean distance (like K-means) but also measure the distance between categorical features using the number of matching categories.
- https://www.kaggle.com/code/rohanadagouda/unsupervised-learning-using-k-prototype-and-dbscan/notebook
    - It calculates Euclidean distances for numerical variables and Similarities for categorical variables
    - The cost is a combined similarity measure for both numeric and categorical variables calculated between the objects and the cluster prototypes

### Model Calibration

- From https://scikit-learn.org/stable/modules/calibration.html
    - The main advantage of `ensemble=True` is to benefit from the traditional ensembling effect. The resulting ensemble should both be well calibrated and slightly more accurate than with `ensemble=False`. 
    - The main advantage of using `ensemble=False` is computational: it... decreases the final model size and increases prediction speed.

### Image Processing

- cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
- https://stackoverflow.com/questions/50963283/python-opencv-imshow-doesnt-need-convert-from-bgr-to-rgb
- https://note.nkmk.me/en/python-opencv-bgr-rgb-cvtcolor/

### HyperLogLog

- https://towardsdatascience.com/hyperloglog-a-simple-but-powerful-algorithm-for-data-scientists-aed50fe47869
- Estimate the number of unique values within a very large dataset using little memory and time

### Classification for Ordered Classes

- https://stats.stackexchange.com/questions/222073/classification-with-ordered-classes

### NN training checklist

- http://karpathy.github.io/2019/04/25/recipe/

### Warn Before Too Late

- For some products, customers would first reveal their intent to churn (e.g. zeroing the account balance, cancelling membership, etc.), and most of them would then churn after a period. A retention model would try to predict the actual churn event.
- Need to measure for most of customers, how long ago did they reveal their intent to churn before the actual churn event, e.g. 3 months.
- We need the model to warn early, because it is too late to intervene after customers revealing their intent to churn (customer already made up their mind to leave).
- We can then create a 3-month gap between each “feature extraction period” and “event occurring window” pair. Each pair is a “vintage”.
- Alternatively, can build a model to predict the event of zeroing the account balance or cancelling membership, etc.

