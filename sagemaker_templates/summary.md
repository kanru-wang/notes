Thoughts:
- having Feature Store when creating Train/Val/Test data is useful
- the debugging report is useful
- SageMaker framework is a Pipeline framework, and won't run until called

<br>
<br>

### Feature transformation with Amazon SageMaker processing job and Feature Store

- from sagemaker.sklearn.processing import SKLearnProcessor
- Use SKLearnProcessor.run()
    - which takes:
        - a SKLearnProcessor instance
        - input raw data s3 path
        - output data (Train, Val, Test) s3 paths
        - data preparation script
        - arguments needed for the data preparation script
    - which does, in a multiprocessing fashion:
        - create FeatureGroup
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

<br>
<br>

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
        - a customized Predictor class for JSON serializer and deserializer, to deserialize JSON data from an inference endpoint
        - Python and PyTorch versions
    - which does:
        - deploy to a certain endpoint, using a certain computing instance

<br>
<br>

### Building a SageMaker Pipeline to train and deploy a BERT-Based text classifier

- from sagemaker.workflow.steps import ProcessingStep, TrainingStep
- ProcessingStep
    - which takes:
        - SKLearnProcessor without .run(some task)
        - TrainingStep
            - which takes:
                - PyTorch object (mentioned above)
                - Train / Val data ProcessingStep (whose input, output and functionality are similar to SKLearnProcessor.run( ) mentioned above)
                - CacheConfig (before executing a step, if there can be found a previous execution of a step using the same arguments, instead of recomputing the step, the pipeline would use the values from the cache)
            - which does:
                - create a training job to train a model
        - Test data ProcessingStep (whose input, output and functionality are similar to SKLearnProcessor.run( ) mentioned above)
        - evaluation script
            - which does:
                - read model from model path
                - read test data
                - tokenizer.encode_plus
                - calculate metrics
                - save output
        - arguments to the evaluation script
        - PropertyFile that saves key metrics that decide how a conditional step should be executed
    - which does:
        - create a step to fit a model and then calculate evaluation metrics
        - will not run until later calling Pipeline.start()

- Use from sagemaker.workflow.condition_step import ConditionStep
    - which takes:
        - RegisterModel
            - which takes:
                - PyTorch object (mentioned above)
                - inference image uri
                - model artifact property from the TrainingStep mentioned above (not trained yet)
                - deployment instance setting
                - approval status (set to pending for now) 
                - PropertyFile model metrics (mentioned above)
            - which does:
                - register the model for deployment
         - CreateModelStep
             - which takes:
                 - Model
                     - which takes:
                         - inference image uri
                         - model artifact property from the TrainingStep mentioned above (not trained yet)
                 - CreateModelInput
                     - which takes:
                         - deployment instance setting
             - which does:
                 - create the model for deployment
         - ConditionGreaterThanOrEqualTo
             - which takes:
                 - the evaluation ProcessingStep (mentioned above)
                 - the PropertyFile model metrics (mentioned above)
                 - minimum accuracy ParameterFloat threshold
    - which does:
        - only register the model if the accuracy, as determined by the evaluation step, exceeded a threshold

- Use from sagemaker.workflow.pipeline import Pipeline
    - which takes:
        - parameters
        - Train / Val data ProcessingStep (mentioned above)
        - TrainingStep (mentioned above)
        - Evaluation ProcessingStep (mentioned above)
        - ConditionStep (mentioned above)
    - which does:
        - start the pipeline by calling .start()

- After retrieving and verifying the metric, use boto3.client().update_model_package() to approve the model for deployment

- Deploy by doing:
    - boto3.client().create_endpoint_config()
        - which takes:
            - EndpointConfigName
            - deployment instance settings
        - which does:
            - config the endpoint
    - boto3.client().create_endpoint()
        - which takes:
            - EndpointName
            - EndpointConfigName
        - which does:
            - create the endpoint

- Use Predictor class to predict. (Notice that unlike above, here we don't use PyTorchModel, so no need use a customized Predictor to encapsulate JSON serializer and deserializer)

<br>
<br>

### From https://course19.fast.ai/deployment_amzn_sagemaker.html

- To serve models in SageMaker, need a script that implements 4 methods: `model_fn`, `input_fn`, `predict_fn` & `output_fn`.
    - The `model_fn` method needs to load the PyTorch model from the saved weights from disk.
    - The `input_fn` method needs to deserialze the invoke request body into an object we can perform prediction on.
    - The `predict_fn` method takes the deserialized request object and performs inference against the loaded model.
    - The `output_fn` method takes the result of prediction and serializes this according to the response content type.
