# Connect Amazon Athena

Your environment(s) must be on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) to use the Amazon Athena connection.

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
