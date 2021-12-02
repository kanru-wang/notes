## Parameter value ranges

- https://xgboost.readthedocs.io/en/latest/parameter.html

## Early stopping

- In a Sklearn cross validation framework (RandomizedSearchCV or GridSearchCV), in terms of XGBClassifier, there is no way to use the validation set (of each fold) as the early stopping eval set. A fixed dataset need to be used as the early stopping eval set, and does not change when training and validating each fold.
    - https://stackoverflow.com/questions/42993550/gridsearchcv-xgboost-early-stopping
    - https://stackoverflow.com/questions/43866284/grid-search-and-early-stopping-using-cross-validation-with-xgboost-in-scikit-lea

- n_estimators (which is the number of boosting rounds) is a scikit-learn version of original version's num_boost_round parameter.
    - https://github.com/dmlc/xgboost/issues/4909
    - https://www.analyticsvidhya.com/blog/2016/03/complete-guide-parameter-tuning-xgboost-with-codes-python/

- Instead of finding the best early_stopping_rounds, I think finding the best n_estimators (i.e. num_boost_round) by trying a lot of values is an alternative way to find "how deep to go". Of course, early stopping won't happen and total training time will be longer.
    - https://blog.cambridgespark.com/hyperparameter-tuning-in-xgboost-4ff9100a3b2f
    - https://datascience.stackexchange.com/questions/58225/xgboost-rounds-is-equal-to-n-estimators
    - Implied by https://discuss.analyticsvidhya.com/t/optimize-n-estimators-using-xgb-cv/67850

- Other ways
    - A wrapper around XGBClassifier should enable using the CV validation set as the early stopping eval set.
    - DIY a cross validation framework.
    - Use the native learning API's xgboost.cv function.

## Handle imbalanced dataset

From: https://xgboost.readthedocs.io/en/latest/tutorials/param_tuning.html

For the training of XGBoost model, there are two ways to improve it.

- If you care only about the overall performance metric (AUC) of your prediction
    - Balance the positive and negative weights via scale_pos_weight
    - Use AUC for evaluation
- If you care about predicting the right probability
    - In such a case, you cannot re-balance the dataset
    - Set parameter max_delta_step to a finite number (say 1) to help convergence

### 1. scale_pos_weight

- From: https://machinelearningmastery.com/xgboost-for-imbalanced-classification/
    - Small Gradient: Small error or correction to the model. Large Gradient: Large error or correction to the model. The scale_pos_weight value is used to scale the gradient for the positive class.
    - Select a good value for scale_pos_weight even improves AUC.

- From: https://stats.stackexchange.com/questions/243207/what-is-the-proper-usage-of-scale-pos-weight-in-xgboost-for-imbalanced-datasets

        scale_pos_weight = count(negative examples)/count(Positive examples)
        In practice, that works pretty well, but if your dataset is extremely unbalanced I'd recommend using something more conservative like:
        scale_pos_weight = sqrt(count(negative examples)/count(Positive examples)) 
        This is useful to limit the effect of a multiplication of positive examples by a very high weight.

- Here it says that scale_pos_weight should be much lower than the recommended value. From: https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/41359

- scale_pos_weight is indeed an alternative to oversampling / undersampling. From: https://towardsdatascience.com/dealing-with-class-imbalanced-datasets-for-classification-2cc6fad99fd9

### 2. max_delta_step

- Useful for imbalanced data.
    - https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/41359
    - https://www.kaggle.com/c/porto-seguro-safe-driver-prediction/discussion/40618
    - https://stats.stackexchange.com/questions/233248/max-delta-step-in-xgboost
    - https://stats.stackexchange.com/questions/387632/running-xgboost-with-highly-imbalanced-data-returns-near-0-true-positive-rate

### 3. sample_weight

- From https://stackoverflow.com/questions/48079973/
    - scale_pos_weight VS sample_weight
- From https://towardsdatascience.com/how-to-handle-multiclass-imbalanced-data-say-no-to-smote-e9a7f393c310
    - scale_pos_weight for binary classification and sample_weights for both binary and multiclass problems.
    
## eval_metric

- The parameter `eval_metric` in XGBClassifier's `fit()` is the evaluation metric for validation data. It is useful when not evaluating the validation dataset (passed by RandomizedSearchCV or GridSearchCV) using `predict_proba()`, but instead using the built-in `evals_result()` to evaluate on data passed to the `eval_set` parameter. From: https://xgboost.readthedocs.io/en/latest/python/python_api.html

        clf = xgb.XGBClassifier()
        clf.XGBClassifier().fit(
            X_train, y_train,
            early_stopping_rounds=10,
            eval_metric="auc",
            eval_set=[(X_test, y_test)]
        )
        clf.evals_result()

- The parameter `eval_metric`'s default value is according to the `objective` parameter (`rmse` for regression, and `error` for classification). From: https://xgboost.readthedocs.io/en/latest/parameter.html

- `eval_metric` is not the loss/cost/objective function; that is specified with the `objective` parameter instead, and it is default to `binary:logistic` for classification problems.

- The same need to specify evaluation metric in RandomizedSearchCV or GridSearchCV is done by specifying `scoring`. From: https://stackoverflow.com/questions/43500893/gridsearchcv-and-xgbclassifier-with-eval-metric-mlogloss

## Regression

- When regression, set `objective` from the default of `reg:squarederror` to `reg:gamma` might be helpful for modelling skewed data (all positive). It assumes gamma regression with log-link. The model output is a mean of gamma distribution. It might be useful, e.g., for modeling insurance claims severity, or for any outcome that might be gamma-distributed. Meanwhile, the `eval_metric` should be set to `gamma-nloglik`.
- See log link: https://www.theanalysisfactor.com/count-models-understanding-the-log-link-function and https://www.theanalysisfactor.com/the-difference-between-link-functions-and-data-transformations/

## Multiclass and multioutput

- https://scikit-learn.org/stable/modules/multiclass.html

## Missing values

Very often XGBoost receives a sparse matrix as input. In an input sparse matrix, zeros are ignored. From my experience, XGBoost converts zero-cells in the sparse matrix into `np.nan`, so having zeros or having `np.nan` in an input sparse matrix is the same for the model.

If we use XGBoost and we have zeros and missing values in a feature, we should fill missing values by for example `-1` instead of by zeros, because all zeros will be converted to `np.nan`, and the model cannot tell which ones are truely zeros and which ones are truely missing values, and the information is therefore wasted.

(I have not experimented if all zeros would be converted to `np.nan`, when the input data is a dataframe or a dense matrix, instead of a sparse matrix.)

When XGBoost splits on a feature, it very often chooses to split on whether it is `np.nan` or not. When doing SHAP analysis, if the training data is in sparse matrix format, we need to convert it to dense matrix which is required by SHAP (currently). Notice that we also need to convert all zero-cells in the sparse matrix into `np.nan`, because we are matching what XGBoost does. These zero-cells are supposed to get the SHAP values of `np.nan`; whereas if they are not converted to `np.nan` (and are still zeros), they will get the SHAP values of "not being `np.nan`".

    X_train_sparse = sklearn_pipeline["feature_generation_section"].transform(X_train_raw)

    X_train_dense = (
        pd.DataFrame.sparse.from_spmatrix(X_train_sparse, columns=column_names)
        .astype(pd.SparseDtype("float", np.nan))
        .sparse.to_dense()
    )

    explainer = shap.TreeExplainer(sklearn_pipeline["xgboost_model"])

    shap_values = explainer.shap_values(X_train_dense)

Reference

- If we leave zeros as zeros when converting a sparse matrix to a dense matrix, it is possible to see all SHAP values of a certain feature will be positive or negative https://github.com/slundberg/shap/issues/553
- SHAP expects a dense matrix as input https://github.com/slundberg/shap/issues/138

