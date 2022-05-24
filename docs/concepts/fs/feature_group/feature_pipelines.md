A feature pipeline is a program that orchestrates the execution of a dataflow graph of data validation, aggregation, transformation, and other feature engineering steps on input data to create and/or update feature values.

With HSFS, you can write feature pipelines in different languages as shown in the figure below.

<img src="/assets/images/concepts/fs/feature-pipelines.svg">

Python is the most widely used framework for feature engineering due to it extensive library support for aggregations (Pandas), data validation (Great Expectations), and transformations (in Scikit-Learn, TensorFlow, PyTorch, etc). 

SQL has grown in popularity for performing heavy lifting in feature pipelines - computing aggregates on data - when the input data already resides in a data warehouse. However, SQL is limited in its support for data validation (Great Expectations support in DBT is one way to do data validation for SQL) and transformations. Spark provides scalable, general-purpose support for feature engineering, with aggregations, data validation (Great Expecations and Deequ, for example), and transformations. Flink is used for feature engineering when you need very fresh features computed in real-time. 

### Alerting

HSFS also supports alerts, that can be triggered when there are problems in your feature pipelines, for example, when a write fails due to an error or a failed expectation. You can send alerts to different alerting endpoints, such as email or Slack, that can be configured in the Hopsworks UI.

