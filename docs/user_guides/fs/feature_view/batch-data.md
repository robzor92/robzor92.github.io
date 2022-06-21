# Batch data

## Creation
It is very common that ML models are deployed in a "batch" setting where ML pipelines score incoming new data in a regular interval, for example, daily or weekly. Feature view supports batch prediction by returning batch data in a time range specified by `start_time` and `end_time` as dataframe  which can be fed to prediction models subsequently.

=== "Python"
    ```python
    # get batch data
    df = feature_view.get_batch_data(
        start_time = "20220620",
        end_time = "20220627"
    ) # return a dataframe
    ```
=== "Java"
    ```java
    Dataset<Row> ds = featureView.getBatchData("20220620", "20220627")
    ```

## Creation with transformation
If you have specified transformation functions when creating a feature view, you can get back transformed batch data as well. If your transformation functions require statistics of training dataset, you need to provide a training data version beforehand. `init_batch_scoring` will then fetch the statistics and initiate the functions with required statistics. Then you can follow the above examples and create the batch data. Please note that transformed batch data can only be returned in python client but not java client.

```python
feature_view.init_batch_scoring(training_dataset_version=1)
```

