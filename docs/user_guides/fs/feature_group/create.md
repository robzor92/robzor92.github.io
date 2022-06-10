# How to create a Feature Group 

### Introduction

In this guide you will learn how to create and register a feature group with Hopsworks. This guide covers creating a feature group using the HSFS APIs as well as the user interface.

## Prerequisites

Before you begin this guide we suggest you read the [Feature Group](../../../concepts/fs/feature_group/fg_overview.md) concept page to understand what a feature group is and how it fits in the ML pipeline.

## Create using the HSFS APIs

To create a feature group using the HSFS APIs, you need to provide a Pandas or Spark DataFrame. The DataFrame will contain all the features you want to register within the feature group, as well as the primary key, event time and partition key.

### Step 1: Create the metadata object

The first step to create a feature group is to create the API metadata object representing a feature group. Using the HSFS API you can execute:


```python
fg = feature_store.create_feature_group(name="weather",
    version=1,
    description="Weather Features",
    online_enabled=True,
    time_travel_format="HUDI",
    primary_key=['location_id'],
    partition_key=['day'],
    event_time='event_time'
    )
```


The full method documentation is available [here](). `name` is the only mandatory parameter of the `create_feature_group` and represents the name of the feature group. 
In the example above we created the first version of a feature group named *weather*, we provide a description to make it searchable to the other project members as well as making the feature group available online. We specify the `time_travel_format` to be "HUDI" which instructs the HSFS APIs to store the feature data on the offline storage using [Apache Hudi](https://hudi.apache.org/). Hopsworks uses Hudi to provide the [data versioning](../../../concepts/fs/feature_group/versioning.md) capability.

Additionally we specify which columns of the DataFrame will be used as primary key, partition key and event time. Composite primary key and multi level partitioning is also supported. 

The version number is also not required, if you don't specify the version number by default it will 

### Step 2: Register the metadata and save the feature data

The snippet above only created the metadata object on the Python interpreter running the code. To register the feature group metadata and to save the feature data with Hopsworks, you should invoke the `save` method:

```python 
fg.save(df)
```

The save method takes in input a Pandas or Spark DataFrame. HSFS will use the DataFrame columns and types to determine the name and types of features, primary key, partition keyand event time. 
The DataFrame *must* contains the columns specified as primary keys, partition key and event time in the `create_feature_group` call.

If a feature group is online enabled, the `save` method will store the feature data to both the online and offline storage.


## Create a feature group with streaming API enabled

By default, the feature group does not have not have the [stream API](../../../concepts/fs/feature_group/write_apis/#stream-api) enabled. To enable the stream API on a feature group, you should specify the `stream=True` parameter in the `create_feature_group` method:

```python
fg = feature_store.create_feature_group(name="weather",
    version=1,
    description="Weather Features",
    online_enabled=True,
    time_travel_format="HUDI",
    primary_key=['location_id'],
    partition_key=['day'],
    event_time='event_time',
    stream=True,
    )

fg.save(df)
```

## Create using the UI

You can also create a new feature group through the UI.