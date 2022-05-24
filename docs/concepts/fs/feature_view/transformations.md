Hopsworks feature store helps prevent **training-serving skew**, where different feature transformation functions are used when creating training data and when serving features online, by providing transformation functions that can be attached to features in a feature view. When training data is created with a feature view or when a feature vector is retrieved from a feature view, HSFS ensures that any transformation functions defined over any features in the feature view will be applied before returning feature values.

<img src="/assets/images/concepts/mlops/flywheel-advanced.svg">

Transformation functions are Python UDFs (user-defined functions), and each transformation function takes a feature values as input, with a well-defined input type, and returns an output based on the well-defined output schema. There are built-in transformation functions in HSFS, such as minmax_scalar, and you can write your own custom transformation functions as Python UDFs.
