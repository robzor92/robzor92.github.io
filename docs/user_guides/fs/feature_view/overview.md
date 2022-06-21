# Feature View

Feature view is a collection of feature groups and supports all common use cases of model development and deployment. You can create [training datasets](training-data.md), create [batch data](batch-data.md) and get [feature vectors](feature-vectors.md).

If you want to understand more about the concept of feature view, you can refer to [here](../../../concepts/fs/feature_view/fv_overview.md).

## Creation
[Query](./query.md) and [transformation function](./transformation-function.md) are the building blocks of feature view. You can define your set of features by `query`. You can also define `labels`, which is useful when you create training datasets for classification tasks. Furthermore, in python client, each features can be attached by a transformation function which can for example performs feature normalisation.

=== "Python"

    ```python
    # create a simple feature view
    feature_view = feature_store.create_feature_view(
        name='transactions_view',
        query=query
    )
    
    # create a feature view with transformation and label
    feature_view = feature_store.create_feature_view(
        name='transactions_view',
        query=query,
        labels=["fraud_label"],
        transformation_functions={"feature_name": transformation_function}
    )
    ```

=== "Java"

    ```java
    // create a simple feature view
    FeatureView featureView = featureStore.createFeatureView()
                                            .name("transactions_view)
                                            .query(query)
                                            .build();

    // create a feature view with label
    FeatureView featureView = featureStore.createFeatureView()
                                            .name("transactions_view)
                                            .query(query)
                                            .labels(Lists.newArrayList("fraud_label")
                                            .build();
    ```

You can refer to [query](./query.md) and [transformation function](./transformation-function.md) for creating `query` and `transformation_function`.

## Retrieval
Once you have created a feature view, you can retrieve it by specifying a name and a version.

=== "Python"
    ```python
    feature_view = feature_store.get_feature_view(name="transactions_view", version=1)
    ```
=== "Java"
    ```java
    FeatureView featureView = featureStore.getFeatureView("transactions_view", 1)
    ```

## Deletion
If there are some feature view instances which you do not use anymore, you can delete a feature view. It is important to mention that all training datasets (include all materialised hopsfs training data) will be deleted along with the feature view.

=== "Python"
    ```python
    feature_view.delete()
    ```
=== "Java"
    ```java
    featureView.delete()
    ```

## Tags

Feature view also supports tags. You can attach, get and remove tags. You can refer to [here]() if you want to learn more about how tags work.

=== "Python"
    ```python
    # attach
    feature_view.add_tag(name="tag_schema", value={"key", "value"}
    
    # get
    feature_view.get_tag(name="tag_schema")
    
    #remove
    feature_view.delete_tag(name="tag_schema")
    ```
=== "Java"
    ```java
    // attach
    Map<String, String> tag = Maps.newHashMap();
    tag.put("key", "value");
    featureView.addTag("tag_schema", tag)

    // get
    featureView.getTag("tag_schema")

    // remove
    featureView.deleteTag("tag_schema")
    ```

## Next
Once you have created a feature view, next we can [create training data](./training-data.md)