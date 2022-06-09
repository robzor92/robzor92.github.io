# How To Use AWS IAM Roles on EC2 instances

## Introduction

When deploying Hopsworks on EC2 instances you might need to assume different roles to access resources on AWS. 
These roles can be configured in AWS and mapped to a project in Hopsworks.

## Prerequisites
Before you begin this guide you'll need the following:

- A Hopsworks cluster running on EC2.
- [Role chaining](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-role-chaining) setup in AWS.
- Configure role mappings in Hopsworks. For a guide on how to configure this see [AWS IAM Role Chaining](admin/iamRoleChaining.md).

## UI
In this guide, you will learn how to use a mapped IAM role in your project.

### Step 1: Navigate to your project's IAM Role Chaining tab

In the _Project Settings_ page you can find the _IAM Role Chaining_ section showing a list of all IAM roles mapped to your project.

<figure>
  <a href="../../../../assets/images/guides/iam_role/project-settings.png">
    <img src="../../../../assets/images/guides/iam_role/project-settings.png" alt="Role Chaining"/>
  </a>
  <figcaption>Role Chaining</figcaption>
</figure>

### Step 2: Assign default IAM roles to groups in you project

Setting a default will allow you to call _assume\_role_ in code without specifying a role ARN.

You can assign (if you are a Data owner in a project) a default role to a role in your project by clicking on the _default_ 
checkbox of a role mapping. You can set one default per project role. If a default is set for 
a project role (Data scientist or Data owner) and all members (ALL) the default set for the project role will take 
precedence over the default set for all members.

## Code

You can then use the [Hops python library](https://hops-py.logicalclocks.com/) and 
[Hops java/scala library](https://github.com/logicalclocks/hops-util) to assume the roles listed in your _Project Settings_ -> 
_IAM Role Chaining_ page.

When calling _assume\_role_ you can pass the role ARN string or use the get role method that takes the role id 
as an argument. If you assign a default role for your project you can call _assume\_role_ without arguments.

###### python
```python
from hops.credentials_provider import get_role, assume_role
credentials = assume_role(role_arn=get_role(1))
spark.read.csv("s3a://resource/test.csv").show()
```

###### scala
```scala
import io.hops.util.CredentialsProvider
val creds = CredentialsProvider.assumeRole(CredentialsProvider.getRole(1))
spark.read.csv("s3a://resource/test.csv").show()
```

The _assume\_role_  method sets spark hadoop configurations that will allow spark to read s3 buckets. The code examples 
above show how to read s3 buckets using Python and Scala.

The method also sets environment variables **AWS_ACCESS_KEY_ID**, **AWS_SECRET_ACCESS_KEY** and 
**AWS_SESSION_TOKEN** so that programs running in the container can use the credentials for the newly assumed role.

## Conclusion
In this guide you learned how to use IAM roles on a cluster deployed on an EC2 instances.