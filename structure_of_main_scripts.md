### Overall

- All parameters/constants stay in YAML and are imported using `PARAMS = yaml.safe_load()` then `**PARAMS['parms_for_this_function']`
- Insert logger at every important function

### Train

- Get data
    - Pull training data dataframe
    - Select useful columns
- Train model
    - Specify feature engineering Sklearn Transformer objects
    - Specify the model
    - Specify the Sklearn pipeline
    - pipeline.fit
- Save model
- Validate model
    - Pull holdout data dataframe
    - Select useful columns
    - Predict and calculate metrics

### Inference

- Turn payload data from JSON to dataframe
- Process features (doing what's not covered by feature engineering/processing steps in Sklearn pipeline)
- Load model from pickle
- Predict