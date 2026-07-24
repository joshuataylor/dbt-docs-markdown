# Connect Amazon Athena

dbt platformⓘ

Your environment(s) must be on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) to use the Amazon Athena connection.

Connect dbt to Amazon's Athena interactive query service to build your dbt project. The following are the required and optional fields for configuring the Athena connection:

| Field                         | Option                | Description                                                                                      | Type    | Required? | Example               |
| ----------------------------- | --------------------- | ------------------------------------------------------------------------------------------------ | ------- | --------- | --------------------- |
| AWS region name               | region\_name          | AWS region of your Athena instance                                                               | String  | Required  | eu-west-1             |
| Database (catalog)            | database              | Specify the database (Data catalog) to build models into (lowercase only)                        | String  | Required  | awsdatacatalog        |
| AWS S3 staging directory      | s3\_staging\_dir      | S3 location to store Athena query results and metadata                                           | String  | Required  | s3://bucket/dbt/      |
| Athena workgroup              | work\_group           | Identifier of Athena workgroup                                                                   | String  | Optional  | my-custom-workgroup   |
| Athena Spark workgroup        | spark\_work\_group    | Identifier of Athena Spark workgroup for running Python models                                   | String  | Optional  | my-spark-workgroup    |
| AWS S3 data directory         | s3\_data\_dir         | Prefix for storing tables, if different from the connection's s3\_staging\_dir                   | String  | Optional  | s3://bucket2/dbt/     |
| AWS S3 data naming convention | s3\_data\_naming      | How to generate table paths in s3\_data\_dir                                                     | String  | Optional  | schema\_table\_unique |
| AWS S3 temp tables prefix     | s3\_tmp\_table\_dir   | Prefix for storing temporary tables, if different from the connection's s3\_data\_dir            | String  | Optional  | s3://bucket3/dbt/     |
| Poll interval                 | poll\_interval        | Interval in seconds to use for polling the status of query results in Athena                     | Integer | Optional  | 5                     |
| Query retries                 | num\_retries          | Number of times to retry a failing query                                                         | Integer | Optional  | 3                     |
| Boto3 retries                 | num\_boto3\_retries   | Number of times to retry boto3 requests (for example, deleting S3 files for materialized tables) | Integer | Optional  | 5                     |
| Iceberg retries               | num\_iceberg\_retries | Number of times to retry iceberg commit queries to fix ICEBERG\_COMMIT\_ERROR                    | Integer | Optional  | 0                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Development credentials[​](#development-credentials "Direct link to Development credentials")

Enter your *development* (not deployment) credentials with the following fields:

| Field                 | Option                   | Description                                                                | Type    | Required | Example                                  |
| --------------------- | ------------------------ | -------------------------------------------------------------------------- | ------- | -------- | ---------------------------------------- |
| AWS Access Key ID     | aws\_access\_key\_id     | Access key ID of the user performing requests                              | String  | Required | AKIAIOSFODNN7EXAMPLE                     |
| AWS Secret Access Key | aws\_secret\_access\_key | Secret access key of the user performing requests                          | String  | Required | wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY |
| Schema                | schema                   | Specify the schema (Athena database) to build models into (lowercase only) | String  | Required | dbt                                      |
| Threads               | threads                  |                                                                            | Integer | Optional | 3                                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Temporary credentials[​](#temporary-credentials "Direct link to Temporary credentials")

If you prefer to not store long-lived IAM user AWS Access Key ID and AWS Secret Access Key in the dbt platform, you can use `aws_session_token`, which is part of [temporary AWS Security Token Service (STS) credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html), instead. This approach mirrors a user or role’s long-term permissions.

To use temporary credentials:

1. Enter the **AWS Access Key ID** and **AWS Secret Access Key** in the [development environment settings](https://docs.getdbt.com/docs/dbt-platform-environments.md#create-a-development-environment) in the dbt platform user interface (UI).

2. Since the `aws_session_token` isn't available as a UI field in dbt platform, add it using [Extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) in the environment settings or [dbt platform profiles](https://docs.getdbt.com/docs/platform/about-profiles.md).

   When you set the `aws_session_token` in **Extended attributes**, the key is case-sensitive and must be exactly `aws_session_token`.

   For example, you can use an [environment variable](https://docs.getdbt.com/docs/build/environment-variables.md) so the token isn't stored in the text box UI:

   ```yaml
   aws_session_token: '{{ env_var(''DBT_ENV_AWS_SESSION_TOKEN'') }}'
   ```

3. [Assign](https://docs.getdbt.com/docs/build/environment-variables.md#setting-environment-variables) `DBT_ENV_AWS_SESSION_TOKEN` for each environment. Since temporary credentials expire, you need to refresh your STS credentials and update the environment (or the variable value) before expiration.
