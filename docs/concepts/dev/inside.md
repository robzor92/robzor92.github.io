Hopsworks provides a Jupyter notebook development environment for programs written in Python, Spark, Flink, and SparkSQL. You can also develop in your IDE (PyCharm, IntelliJ, etc), test locally, and then run your programs as Jobs in Hopsworks. Jupyter notebooks can also be run as Jobs.

<img src="/assets/images/concepts/dev/dev-inside.svg">


Hopsworks provides source code control inside the platform using Git (Github, Gitlab) or Bitbucket.

In Hopsworks, you can install Python libraries using PyPi, Conda, Wheel files, or Github URLs. The Python libraries are installed in a Conda environment associated with your project. Docker images are compiled transparently for you when you update your Conda enviornment - here is no need for developers to write Dockerfiles, they can just install Python libraries to customize their development or production environments.

In Hopsworks, a Job is a schedulable program that can is allocated resources. You can run a Job in Hopsworks 

* from the UI;
* programmatically with the Hopsworks SDK (Python, Java) or REST API;
* from Airflow programs (either inside our outside Hopsworks);
* from your IDE using a plugin ([PyCharm/IntelliJ plugin](https://plugins.jetbrains.com/plugin/15537-hopsworks));

Airflow comes out-of-the box with Hopsworks, but you can also use an external Airflow cluster (with the Hopsworks Job operator) if you have one. Airflow can be used to schedule the execution of Jobs, individually or as part of bigger DAGs.
