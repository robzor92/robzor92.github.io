Hopsworks Model Registry enables developers to publish, test, monitor, govern and share models for collaboration with other teams. The model registry is where developers publish their models during the experimentation phase. The model registry can also be used to share models with the team and stakeholders.

<img src="/assets/images/concepts/mlops/model-registry.svg">

The model registry centralizes model management, enabling models to be securely accessed and governed. Models are more than just the model itself - the registry also stores sample data for testsing, configuration information, provenance information, environment variables, links to the code used to generate the model, the model version, and tags/descriptions). 

## Model Package

 - Model Schema
 - Model artifacts
 - Model version information
 - Model format (based on the ML framework used to train the model - e.g., .pkl or .tb files)

You can also optionally include in your packaged model:
 - Sample data (used to test the model  in KServe)
 - The source notebook/program/experiment used to create the model


