A feature vector is a row of features (without the primary key(s) and event timestamp):

<img src="../../../../assets/images/concepts/fs/feature-vector.svg">

The Feature View's online API provides methods to return an individual feature vector, or a batch of feature vectors, containing the latest feature values, given a valid primary key for the feature view.

It may be the case that some features should not be retrieved from the feature store, but are supplied by the client. We call these 'passed' features and, similar to precomputed features from the feature store, they can also be transformed by the HSFS client in the method:

* feature_view.get_feature_vector(<primary-keys>, passed={ .... })

