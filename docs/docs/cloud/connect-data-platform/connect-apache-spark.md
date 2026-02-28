# Connect Apache Spark

If you're using Databricks, use `dbt-databricks`

If you're using Databricks, the `dbt-databricks` adapter is recommended over `dbt-spark`. If you're still using dbt-spark with Databricks consider [migrating from the dbt-spark adapter to the dbt-databricks adapter](https://docs.getdbt.com/guides/migrate-from-spark-to-databricks.md).

For the Databricks version of this page, refer to [Databricks setup](#databricks-setup).

note

See [Connect Databricks](#connect-databricks) for the Databricks version of this page.

dbt supports connecting to an Apache Spark cluster using the HTTP method or the Thrift method. Note: While the HTTP method can be used to connect to an all-purpose Databricks cluster, the ODBC method is recommended for all Databricks connections. For further details on configuring these connection parameters, please see the [dbt-spark documentation](https://github.com/dbt-labs/dbt-spark#configuring-your-profile).

To learn how to optimize performance with data platform-specific configurations in dbt, refer to [Apache Spark-specific configuration](https://docs.getdbt.com/reference/resource-configs/spark-configs.md).

The following fields are available when creating an Apache Spark connection using the HTTP and Thrift connection methods:

| Field                 | Description                                                     | Examples                |
| --------------------- | --------------------------------------------------------------- | ----------------------- |
| Host Name             | The hostname of the Spark cluster to connect to                 | `yourorg.sparkhost.com` |
| Port                  | The port to connect to Spark on                                 | 443                     |
| Organization          | Optional (default: 0)                                           | 0123456789              |
| Cluster               | The ID of the cluster to connect to                             | 1234-567890-abc12345    |
| Connection Timeout    | Number of seconds after which to timeout a connection           | 10                      |
| Connection Retries    | Number of times to attempt connecting to cluster before failing | 10                      |
| User                  | Optional                                                        | dbt\_cloud\_user        |
| Auth                  | Optional, supply if using Kerberos                              | `KERBEROS`              |
| Kerberos Service Name | Optional, supply if using Kerberos                              | `hive`                  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

[![Configuring a Spark connection](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/spark-connection.png?v=2 "Configuring a Spark connection")](#)Configuring a Spark connection

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
