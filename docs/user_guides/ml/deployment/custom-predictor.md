

To configure a custom predictor, users must provide a python script implementing the following class.

=== "Python"

    ``` python
    class Predict(object):

        def __init__(self):
            """ Initialization code goes here:
                - Download the model artifact
                - Load the model
            """
            pass

        def predict(self, inputs):
            """ Serve predictions using the trained model"""
            pass
    ```




The predictor script should be available via a local file system path or a path on HopsFS. The path to this script then has to be provided when calling `deploy()` or `create_predictor()` methods. Find more details in the [Predictor Reference](predictor_api.md).
