# Data Validation

Reliable data is of paramount importance to push ML/AI models into production. Hopsworks offers integration with Great Expectations to enable a smooth data validation workflow. This guide is designed to help you integrate a data validation step when inserting new dataframes into a feature group. Note that validation is performed on the client machine, not in the backend.

# Code 

## Setup

In order to follow along you will need:
- A dataframe to validate
- A Hopsworks project
- great_expectations installed 

### Dataframe to validate

Here is a small dataframe to validation. You can also choose to import your own data.

```python3
import pandas as pd

df = pd.DataFrame({
    "foo_id": [1, 2, 3, 4, 5], 
    "bar_name": ["alice", "bob", "carl", "dylan", "e"]
})
```

### Hopsworks pre-requisite

 You can then setup a connection to the featurestore of your hopsworks project. 

```python3
import hsfs

conn = hsfs.connection()
fs = conn.get_feature_store()
```

Before writing data to Hopsworks, we first need to create a featuregroup.

```python3
fg = fs.create_feature_group(
  "fg_with_data_validation",
  version=1,
  description="Validated data",
  primary_key=['foo_id'],
  online_enabled=False
)

fg.save(df)
```

### Great Expectations pre-requisite

Create (or import an existing) expectation suite using the Great Expectations library.

```python3
import great_expectations as ge

expectation_suite = ge.core.ExpectationSuite(
    expectation_suite_name="validate_on_insert_suite"
)
```

Add some expectation to your suite to validate column:
```python3
expectation_suite.add_expectation(
    ge.core.ExpectationConfiguration(
        expectation_type="expect_column_minimum_value_to_be_between",
        kwargs={
            "column": "foo_id",
            "min_value": 0,
            "max_value": 1
        }
    )
)

expectation_suite.add_expectaion(
    ge.core.ExpectationConfiguration(
        expectation_type="expect_column_value_lengths_to_be_between",
        kwargs={
            "column": "bar_name",
            min_value: 3,
            max_value: 10
        }
    )
)
```

## Integrating Great Expectations with Hopsworks

Hopsworks implements different support function to make the integration of a data validation step in your data engineering pipeline. By default, validation objects returned by Hopswors are native great_expectations object you can use directly. Hopsworks aims to avoid as much as possible to force you to deal with additional abstractions. You can access the underlying Hopsworks abstractions via by setting `python3 native=False`.

### Attach an Expectation Suite to a Featuregroup

The first step is to attach an expectation suite to your featuregroup. It enables persistence of the expectation suite in the hopsworks backend.

```python3
fg.save_expectation_suite(expectation_suite)

# or directly when creating your featuregroup

fg = fs.create_feature_group(
    ...,
    expectation_suite=expectation_suite
)
```

This suite can then easily be retrieved or deleted whenever you are working with this featuregroup by calling:

```python3
ge_expectation_suite = fg.get_expectation_suite()
# or
fg.drop_expectation_suite()
```

When fetching an expectation suite from Hopsworks the meta field of each expectation contained in the suite is populated with an `python3 expectationId` field. This id is used in the backend to link a particular expectation to its validation historic. Do not edit it directly.

### Validate your data

As validation objects returned by Hopsworks are native Great Expectation object you can run validation using the usual Great Expectations syntax:

```python3
ge_df = ge.from_pandas(df, expectation_suite=fg.get_expectation_suite())
ge_report = ge_df.validate()
```

or use the convenient wrapper method provided by Hopsworks featuregroup:

```python3
ge_report = fg.validate(df)
```

This will run the validation using the expectation suite attached to this featuregroup and raise an exception if no attached suite is found.


### Save Validation Reports

When running validation using Great Expectations, a validation report is generated containing all validation results for the different expectations. Each result provides informations about whether the provided dataframe conforms to the corresponding expectation. These reports can be stored in Hopsworks to save a validation history for the data written to a particular featuregroup. 

```python3
fg.save_validation_report(ge_report)
```

A summary of these reports will then be available in the Hopsworks web interface enabling easy monitoring. For in-depth analysis, it is possible to download the complete report from the web interface or retrieve it via the hsfs client.

```python3
validation_history = fg.get_validation_reports()

# or a convenience method for rapid development

ge_latest_report = fg.get_latest_validation_report()
```

## Data validation in development or production environments

Depending on your context, you might want to use (or not use) data validation in different ways. Hopsworks aims to provide both a smooth development experience as well as an easy and robust path to a production pipeline. This is achieved through two key mechanisms:

- Validation On Insertion
- Monitoring Or Gatekeeping

### Validation On Insertion

By default, attaching an expectation suite to a featuregroup enables automatic validation on insertion. Meaning calling `fg.insert` after attaching an expectation suite to a featuregroup via `fg.save_expectation_suite` will perform validation under the hood (on the client machine) and upload the validation report. This approach enables developper to write cleaner more maintainable code as well as keeping your data validation history in the same place as the data itself.

In your expectation suite script:

```python3
expectation_suite = ge.core.ExpectationSuite(
    expectation_suite_name="validate_on_insert_suite"
)

expectation_suite.add_expectation(
    ge.core.ExpectationConfiguration(
        expectation_type="expect_column_minimum_value_to_be_between",
        kwargs={
            "column": "foo_id",
            "min_value": 0,
            "max_value": 1
        }
    )
)

# run_validation kwarg defaults to True
fg.save_expectation_suite(expectation_suite, run_validation=True)
```

In your insertion script:

```python3
# Clean and Simple
fg.insert(df)

# Instead of a more cumbersome approach
expectation_suite_path = Path("./my_expectation_suite.json")
report_path = Path("./my_validation_report.json")

with expectation_suite_path.open("r") as f:
    expectation_suite = json.load(expectation_suite_path)

ge_df = ge.from_pandas(df, expectation_suite=expectation_suite)
report = ge_df.validate()

with report_path.open("w") as f:
    json.dumps(f, report.to_json())

fg.insert(df)
```

In addition of minimizing boilerplate in insertion jobs, you can get a summary of the validation reports for each insertion.

There is a variety of use cases where performing data validation on insertion is not desirable, e.g. during a project early development or to backfill large amount of pre-validated data for a time-sensitive project deadline. In those cases, `fg.insert` implements a run_validation kwarg that overrides the registered behaviour:

```python3
fg.insert(df, validation_kwargs={"run_validation": False})
```

### Monitoring or Gatekeeping

Data validation steps in a data engineering pipeline has two complementary use cases, monitoring and gatekeeping. In the first case, one wishes to set up validation as a reporting tool. The aim is to gather metrics on the ingested data and create a history that can inform the user about the evolution of certain trends in the data. This use case is typical in a development setup where the data is still being characterized and reliable quality is not required. Setting it up during development also enables an easier transition towards a production setup. Indeed, it remains useful in production to detect feature drift and log information about incoming data.

On the other hand, production setup often requires additional protection to prevent bad quality data to find their way into the featuregroup. A typical example is preventing feature vector to have a NaN values which could lead to issue at inference time. In such cases data validation can be used as a gatekeeper to prevent erroneous data to find their way into an online feature store.

Hopsworks is focused on making the transition from development to production as seamless as possible. To switch between these two behaviours one can simply use the `insertion_policy` kwarg. By default, expectation suite are attached to featuregroups as a monitoring tool. This default choice is made as it corresponds to development setup and avoids any loss of data on insertion.

```python3
fg.save_expectation_suite(expectation_suite)
# defaults to the monitoring behaviour
fg.save_expectation_suite(expectation_suite, insertion_policy="ALWAYS")
```

When your development cycle is nearing the end and you want to check the production behaviour you can simply change to gatekeeping by setting:

```python3
fg.save_expectation_suite(fg.get_expectation_suite(), insertion_policy="STRICT")
```



