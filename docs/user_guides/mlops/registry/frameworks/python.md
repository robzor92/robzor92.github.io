# How To Export a Python Model

## Introduction

In this guide you will learn how to export a Python model.

## Code

### Step 1: Connect to Hopsworks

```python
import hopsworks

connection = hopsworks.connection()

project = connection.get_project("my_project")

# get Hopsworks Model Registry handle
mr = project.get_model_registry()
```

### Step 2: Train

### Step 3: Create

### Step 3: Save

## Conclusion

In this guide you learned how to...