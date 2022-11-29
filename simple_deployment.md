### Batch

Airflow orchestration tool can implement:
- Time based scheduling
- Inter-job dependencies
- Start an AWS ECR container or EC2 instance
- Integrate CloudWatch for logging and monitoring
- The following 3 points (which can also be done in Ansible)
    - Pull and pre-processing data
    - Run model inference
    - Output post-processing

### Near Real Time

- A Python script to start an API server (e.g. using Flask Connexion package)
- A Python function to turn payload data from JSON to pd.DataFrame, replace empty values with None, and run inference.
- An OpenAPI (i.e. Swagger) file to describe the API
    - Define the API endpoints (Flask) i.e. what are the response codes, 200, 400, 401...
    - URL of the endpoint
    - Validate the JSON schema of inputs/outputs
- Code saved to Artifactory
- During inference:
    - Business application passes an identity access token to a Kong API endpoint
    - Kong API endpoint triggers the model (Kong uses OpenAPI/Swagger files)

For both Batch and Near Real Time, the model inference and data pre/post-processing can be run in a docker container running on AWS ECS.
