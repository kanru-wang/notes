### From: https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html

One key thing about Pipeline class is the special meaning of its functions:

- fit means first fit_transform, then use the final estimator to fit
- fit_transfrom means first fit_transform, then use the final estimator to fit_transform
- predict means first transform, then use the final estimator to predict
- predict_proba means first transform, then use the final estimator to predict_proba

### From: https://scikit-learn.org/0.19/auto_examples/hetero_feature_union.html

Remember you can also plugin the transformer objects fitted or model objects trained in Jupyter notebooks.

### From: https://scikit-learn.org/stable/modules/compose.html

- With Pipeline, the cross validation (using sklearn's Grid Search or Random Search) can be done on hyper-parameters not only in the model, but in any data pre-processing functions as well.
- Notice the "__" which facilitates parameters passing through.
- Can retrieve every step of the pipeline individually.

### TFIDF Transformer

- TfidfVectorizer has an attribute `idf_` meaning that TfidfVectorizer remembers the IDF part from the training dataset.
- When `transform()` is called, it uses the vocabulary and document frequencies (`DF`) learned by fit (or fit_transform) to transform documents to document-term matrix.
- In other words, during inference, the `TF` part is observed from the new text, the `IDF` part is still using the values derived from training dataset.