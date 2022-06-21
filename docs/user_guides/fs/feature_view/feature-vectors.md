# Feature Vectors
Once you have trained a model, it is time to deploy it. You can get back all the features required to feed into an ML model with a single method call. Feature view provides great flexibility for you to get the features from either inside the Hopsworks platform or an external environment, for example, your application server. Harnessing the powerful [RonDB](https://www.rondb.com/), feature vectors are served in just a few milliseconds.

If you want to understand more about the concept of feature vectors, you can refer to [here](../../../concepts/fs/feature_view/online_api.md).

## Retrieval
You can get back feature vectors from either python or java client by providing the primary key and value of the entry you need.

=== "Python"
    ```python
    # get a single vector
    feature_view.get_feature_vector(
        entry = {"pk1": 1, "pk2": 2}
    )

    # get multiple vectors
    feature_view.get_feature_vectors(
        entry = [
            {"pk1": 1, "pk2": 2},
            {"pk1": 3, "pk2": 4},
            {"pk1": 5, "pk2": 6}
        ]
    )
    ```
=== "Java"
    ```java
    // get a single vector
    Map<String, Object> entry1 = Maps.newHashMap();
    entry1.put("pk1", 1);
    entry1.put("pk2", 2);
    featureView.getFeatureVector(entry1);

    // get multiple vectors
    Map<String, Object> entry2 = Maps.newHashMap();
    entry2.put("pk1", 3);
    entry2.put("pk2", 4);
    featureView.getFeatureVectors(Lists.newArrayList(entry1, entry2);
    ```

### Retrieval with transformation
If you have specified transformation functions when creating a feature view, you can get back transformed feature vectors as well. If your transformation functions require statistics of training dataset, you need to provide a training data version beforehand. `init_serving` will then fetch the statistics and initiate the functions with required statistics. Then you can follow the above examples and retrieve the feature vectors. Please note that transformed feature vectors can only be returned in python client but not java client.

```python
feature_view.init_serving(training_dataset_version=1)
```

## Preview
In order to enable ML engineers to test feature serving easily, feature view returns a sample of feature vectors without specifying any primary keys.
=== "Python"
    ```python
    # get a single vector
    feature_view.preview_feature_vector()

    # get multiple vectors
    feature_view.get_feature_vectors(n=3) # n = size of feature vectors
    ```
=== "Java"
    ```java
    // get a single vector
    featureView.previewFeatureVector();

    // get multiple vectors
    featureView.previewFeatureVectors(3);
    ```

## Passed features
fabio's part
