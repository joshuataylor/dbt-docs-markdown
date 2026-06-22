# Connect Apache Spark to Fusion [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

The dbt Fusion engine supports Apache Spark, enabling faster compilation and execution for your Spark-based dbt projects. Currently, Fusion only supports Apache Spark 3.0.

## Fusion and Spark[​](#fusion-and-spark "Direct link to Fusion and Spark")

Fusion uses the Databricks SQL dialect for [static analysis](https://docs.getdbt.com/docs/fusion/new-concepts.md#principles-of-static-analysis) when working with Spark. Databricks SQL is a superset of Spark SQL, so your SQL is validated with Databricks semantics. This provides comprehensive error checking and SQL comprehension features. A dedicated Spark SQL dialect for static analysis is planned for a future release.

## Authentication[​](#authentication "Direct link to Authentication")

The Spark adapter in Fusion supports:

* Thrift

  <!-- -->

  * Simple Authentication and Security Layer (SASL) PLAIN
  * No SASL (NOSASL)

* Livy

  <!-- -->

  * Basic authentication (username and password)
  * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
    <!-- -->
    * Supports authentication using single sign-on, service accounts, or user tokens

## Configure Fusion[​](#configure-fusion "Direct link to Configure Fusion")

Configure your Spark connection in `profiles.yml`:

\~/.dbt/profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: spark
      method: CONNECTION_METHOD # thrift, http, or livy
      port: PORT_NUMBER
      auth: AUTH_METHOD
      schema: SCHEMA_NAME
      host: HOSTNAME
      platform_hint: PLATFORM_HINT # optional; aws_emr_serverless or aws_emr_eks
      server_side_parameters: # optional; required keys depend on platform_hint
        livy.server.session.ttl: SESSION_TTL_SECONDS # optional when using method: livy
```

| Profile field            | Required | Description                                                                                                                                                                                                                                                                                                                                                                                           | Example                                         |
| ------------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| `method`                 | Yes      | Connection method. Accepted values: `thrift`, `http`, or `livy`.                                                                                                                                                                                                                                                                                                                                      | `thrift`                                        |
| `port`                   | Yes      | Port number for the connection.                                                                                                                                                                                                                                                                                                                                                                       | `443`                                           |
| `auth`                   | Yes      | Authentication method.                                                                                                                                                                                                                                                                                                                                                                                | `SASL PLAIN`, `NOSASL`, `AWS_SIGV4`             |
| `schema`                 | Yes      | The database or schema name where dbt will create and query objects.                                                                                                                                                                                                                                                                                                                                  | `analytics`                                     |
| `host`                   | Yes      | Hostname of the Spark cluster or Databricks workspace.                                                                                                                                                                                                                                                                                                                                                | `yourorg.sparkhost.com`                         |
| `platform_hint`          | No       | Hints to Fusion which Spark platform you use. Fusion uses this to validate required `server_side_parameters`. Accepted values: `aws_emr_serverless`, `aws_emr_eks`. If omitted, Fusion assumes a generic Spark cluster.                                                                                                                                                                               | `aws_emr_serverless`                            |
| `server_side_parameters` | No       | Spark session parameters passed to the cluster.<br /><br />Required keys when using `platform_hint`:<br />- For `aws_emr_serverless`, use `emr-serverless.session.executionRoleArn`.<br />- For `aws_emr_eks`, use `spark.kubernetes.namespace`.<br /><br />When using `method: livy`, you can set `livy.server.session.ttl` to configure how long a session can remain idle before it is terminated. | Refer to [example profiles](#example-profiles). |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Example profiles[​](#example-profiles "Direct link to Example profiles")

* Thrift (binary)
* Thrift (HTTP)
* AWS EMR Serverless
* AWS EMR on EKS

\~/.dbt/profiles.yml

```yaml
spark-local-thrift-binary:
  target: spark
  outputs:
    spark:
      type: spark
      method: thrift
      port: 10000
      auth: NOSASL
      host: localhost
      schema: my_schema
```

\~/.dbt/profiles.yml

```yaml
spark-local-thrift-http:
  target: spark
  outputs:
    spark:
      type: spark
      method: http
      port: 443
      auth: NOSASL
      # Omit the protocol scheme to use HTTPS. For HTTP, use a host like http://localhost
      host: localhost
      schema: my_schema
```

\~/.dbt/profiles.yml

```yaml
spark-emr-serverless:
  target: spark
  outputs:
    spark:
      type: spark
      method: livy
      auth: AWS_SIGV4
      port: 443
      host: "YOUR_APPLICATION_ID.livy.emr-serverless-services.us-east-1.amazonaws.com"
      schema: my_schema
      platform_hint: aws_emr_serverless
      server_side_parameters:
        "spark.driver.memory": "1g"
        "spark.driver.cores": 1
        "spark.executor.memory": "1g"
        "spark.executor.cores": 1
        # Required by EMR Serverless when using platform_hint: aws_emr_serverless
        "emr-serverless.session.executionRoleArn": "arn:aws:iam::YOUR_AWS_ACCOUNT:role/YOUR_ROLE"
```

\~/.dbt/profiles.yml

```yaml
spark-emr-eks:
  target: spark
  outputs:
    spark:
      type: spark
      method: livy
      auth: AWS_SIGV4
      port: 443
      host: "https://my-spark-cluster.com"
      schema: my_schema
      platform_hint: aws_emr_eks
      server_side_parameters:
        # Required by EMR on EKS when using platform_hint: aws_emr_eks
        "spark.kubernetes.namespace": "emr-jobs"
        "spark.driver.memory": "1g"
        "spark.driver.cores": 1
        "spark.executor.memory": "8g"
        "spark.executor.cores": 4
```

For detailed configuration options, refer to the [Spark configuration](https://docs.getdbt.com/reference/resource-configs/spark-configs.md) page.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
