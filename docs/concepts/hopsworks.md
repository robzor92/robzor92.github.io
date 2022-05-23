Hopsworks is a **modular** MLOps platform with:

 - a feature store
 - model registry and model serving based on KServe
 - vector database based on OpenSearch
 - a data science and data engineering platform

<img src="/assets/images/concepts/mlops/architecture.svg">

## Standalone Feature Store
Hopsworks was the first open-source and first enterprise feature store for ML.  You can use Hopsworks as a standalone feature store with the HSFS API.

## Model Management
Hopsworks includes support for model management, with a model registry and model deployments based on the open-source KServe framework. 

## Vector Database
Hopsworks provides an embedding store with OpenSearch kNN (FAISS and nmslib), that includes out-of-the-box: authentication, access control, filtering, scalability. You can easily integrate your feature store with OpenSearch to build scalable systems, such as ranking-and-retrieval systems for real-time recommendations. 

## Governance
Hopsworks provides a data-mesh architecture for managing ML assets and teams, with multi-tenant projects. Not unlike a github repository, a project is a sandbox containing team members, data, and ML assets. In Hopsworks, all ML assets (features, models, training data) are versioned, taggable, lineage-tracked, and support free-text search. Data can be also be securely shared between projects.

## Data Science Platform
You can develop feature engineering pipelines and training pipelines in Hopsworks. There is support for version control (github, gitlab, bitbucket), Jupyter notebooks, a shared distriubted file system, per project conda environments for managing python dependencies without needing to write Dockerfiles, jobs (Python, Spark, Flink), and workflow orchestration with Airflow.
