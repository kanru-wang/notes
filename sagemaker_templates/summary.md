### Feature transformation with Amazon SageMaker processing job and Feature Store

- Use from sagemaker.sklearn.processing import SKLearnProcessor
    - which takes:
        - Input raw data s3 path
        - Output data (train, val, test) s3 paths
        - All arguments needed for processing data
    - which does, in a multiprocessing fashion:
        - Create FeatureGroup
            - Use from sagemaker.feature_store.feature_group import FeatureGroup
                - which takes:
                    - FeatureDefinition (the datatype of each feature)
                - which does:
                    - needed for Feature Store
                    - used to create an Athena query
        - read_csv
        - tokenizer.encode_plus
        - sample to balance the dataset
        - Train / Val / Test split
        - save to csv
        - ingest (save) Train / Val / Test data to the Feature Store
        - When "wait" is set to False, will not run. Need to call job.wait() to run

- To retrieve the data from the FeatureGroup in the future, use athena_query().run(query_string, s3_output_location)

### Train a review classifier with BERT and Amazon SageMaker

- Use from sagemaker.pytorch import PyTorch
    - which takes:
        - training script
            - which does:
                - initialize the distributed environment
                - get configuration from RobertaConfig.from_pretrained
                - create model from RobertaForSequenceClassification.from_pretrained
                - create DataLoader
                - create loss function
                - create optimizer
                - epoch for loop
                - save model
        - training instance settings
        - Python and PyTorch versions
        - hyperparameters
        - metric log regex
        - DebuggerHookConfig (s3 path to save debugging output)
        - ProfilerConfig (config for Profiler)
        - Rules for Profiler (see the output in the profiler_report folder)
        - Train / Val data s3 path
    - which does:
        - fit a model
        - When "wait" is set to False, will not run. Need to call job.wait() to run

- Use from sagemaker.pytorch.model import PyTorchModel
    - which takes:
        - inference script:
            - which does:
                - load model
                - tokenize input
                - predict 
        - model name string
        - data
        - a customized class for JSON serializer and deserializer, to deserialize JSON data from an inference endpoint
        - Python and PyTorch versions
    - which does:
        - deploy to a certain endpoint, using a certain computing instance

### From https://course19.fast.ai/deployment_amzn_sagemaker.html

- To serve models in SageMaker, need a script that implements 4 methods: `model_fn`, `input_fn`, `predict_fn` & `output_fn`.
    - The `model_fn` method needs to load the PyTorch model from the saved weights from disk.
    - The `input_fn` method needs to deserialze the invoke request body into an object we can perform prediction on.
    - The `predict_fn` method takes the deserialized request object and performs inference against the loaded model.
    - The `output_fn` method takes the result of prediction and serializes this according to the response content type.
