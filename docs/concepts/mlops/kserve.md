KServe is an open-source framework for model serving on Kubernetes.
In Hopsworks, you can easily deploy models from the model registry in KServe or in Docker containers (for Hopsworks Community). You can deploy model in either programs, using the HSML library, or in the UI. A KServe model deployment can include the following components:

 - **Transformer**
    - A pre-processing and post-processing component that can transform model inputs before predictions are made
 - **Predictor** 
    - A predictor is a ML model in a Python object that takes a feature vector as input and returns a prediction as output
 - **Logger**
    - Hopsworks logs inputs and outputs to transformers and predictors to a Kafka topic that is part of the same project as the model
 - **Versioned Deployments**
    - Model deployments are versioned, enabling A/B testing and more.
 - **Istio Model Endpoint**
    - You can publish model via a REST Endpoint using Istio and access it over HTTP using a Hopsworks API key (with serving scope). Secure and authorized access is guaranteed by Hopsworks.

Models deployed on KServe in Hopsworks can be easily integrated with the Hopsworks feature store using a Transformer Python script, that builds the predictor's input feature vector using the application input and pre-computed features from the feature store.

<img src="/assets/images/concepts/mlops/kserve.svg">
