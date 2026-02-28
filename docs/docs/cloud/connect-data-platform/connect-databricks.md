# Connect Databricks Fusion compatible

The dbt-databricks adapter is maintained by the Databricks team. The Databricks team is committed to supporting and improving the adapter over time, so you can be sure the integrated experience will provide the best of dbt and the best of Databricks. Connecting to Databricks via dbt-spark has been deprecated.

## About the dbt-databricks adapter[​](#about-the-dbt-databricks-adapter "Direct link to About the dbt-databricks adapter")

dbt-databricks is compatible with the following versions of dbt Core in dbt with varying degrees of functionality.

| Feature        | dbt Versions                           |
| -------------- | -------------------------------------- |
| dbt-databricks | Available starting with dbt 1.0 in dbt |
| Unity Catalog  | Available starting with dbt 1.1        |
| Python models  | Available starting with dbt 1.3        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The dbt-databricks adapter offers:

* **Easier set up**
* **Better defaults:** The dbt-databricks adapter is more opinionated, guiding users to an improved experience with less effort. Design choices of this adapter include defaulting to Delta format, using merge for incremental models, and running expensive queries with Photon.
* **Support for Unity Catalog:** Unity Catalog allows Databricks users to centrally manage all data assets, simplifying access management and improving search and query performance. Databricks users can now get three-part data hierarchies – catalog, schema, model name – which solves a longstanding friction point in data organization and governance.

To learn how to optimize performance with data platform-specific configurations in dbt, refer to [Databricks-specific configuration](https://docs.getdbt.com/reference/resource-configs/databricks-configs.md).

To grant users or roles database permissions (access rights and privileges), refer to the [example permissions](https://docs.getdbt.com/reference/database-permissions/databricks-permissions.md) page.

To set up the Databricks connection, supply the following fields:

| Field           | Description                                              | Examples                               |
| --------------- | -------------------------------------------------------- | -------------------------------------- |
| Server Hostname | The hostname of the Databricks account to connect to     | dbc-a2c61234-1234.cloud.databricks.com |
| HTTP Path       | The HTTP path of the Databricks cluster or SQL warehouse | /sql/1.0/warehouses/1a23b4596cd7e8fg   |
| Catalog         | Name of Databricks Catalog (optional)                    | Production                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

[![Configuring a Databricks connection using the dbt-databricks adapter](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/dbt-databricks.png?v=2 "Configuring a Databricks connection using the dbt-databricks adapter")](#)Configuring a Databricks connection using the dbt-databricks adapter

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
