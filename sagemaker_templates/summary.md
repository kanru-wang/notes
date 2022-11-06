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
                    - used to 
                    - used to create an Athena query
        - read_csv
        - tokenizer.encode_plus
        - sample to balance the dataset
        - Train / Val / Test split
        - save to csv
        - ingest (save) Train / Val / Test data to the FeatureGroup

- To retrieve the data from the FeatureGroup in the future, use athena_query().run(query_string, s3_output_location)
