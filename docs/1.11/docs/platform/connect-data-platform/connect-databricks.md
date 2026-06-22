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

## Warehouse permissions for Fusion[​](#warehouse-permissions-for-fusion "Direct link to Warehouse permissions for Fusion")

<!-- -->

The Databricks user or service principal that dbt Fusion engine uses must have privileges on the catalog and schemas where models run, plus access required for metadata queries. Requirements depend on whether you use Unity Catalog or the legacy Hive Metastore.

### Required Databricks objects[​](#required-databricks-objects "Direct link to Required Databricks objects")

Before connecting, these objects must exist or be accessible:

| Object                        | Purpose                                 |
| ----------------------------- | --------------------------------------- |
| **Workspace**                 | Your Databricks workspace URL (host)    |
| **SQL warehouse or cluster**  | Compute resource (using `http_path`)    |
| **Catalog**                   | Unity Catalog or Hive Metastore catalog |
| **Schema**                    | Target schema within the catalog        |
| **User or service principal** | Identity for authentication             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Unity Catalog[​](#unity-catalog "Direct link to Unity Catalog")

Required access for the Unity Catalog:

| Permission      | Object          | Purpose                              |
| --------------- | --------------- | ------------------------------------ |
| `USE CATALOG`   | Catalog         | Access the catalog                   |
| `USE SCHEMA`    | Schema          | Access schemas                       |
| `SELECT`        | Tables or views | Read existing data and sources       |
| `CREATE TABLE`  | Schema          | Create models materialized as tables |
| `CREATE VIEW`   | Schema          | Create models materialized as views  |
| `MODIFY`        | Tables          | Insert, update, and delete data      |
| `CREATE SCHEMA` | Catalog         | Auto-create schemas (if needed)      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Hive Metastore[​](#hive-metastore "Direct link to Hive Metastore")

Required access for the legacy Hive Metastore:

| Permission | Object   | Purpose                 |
| ---------- | -------- | ----------------------- |
| `USAGE`    | Database | Access the database     |
| `SELECT`   | Tables   | Read data               |
| `CREATE`   | Database | Create tables and views |
| `MODIFY`   | Tables   | Modify data             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Metadata operations[​](#metadata-operations "Direct link to Metadata operations")

The following are required for fundamental dbt features:

| Query type                   | SQL used                                             | Required permission        |
| ---------------------------- | ---------------------------------------------------- | -------------------------- |
| Get table schema             | `DESCRIBE TABLE` or `DESCRIBE TABLE EXTENDED`        | SELECT on table            |
| Get table schema (DBR 16.2+) | `DESCRIBE TABLE EXTENDED ... AS JSON`                | SELECT on table            |
| List relations               | Query `INFORMATION_SCHEMA.TABLES`                    | USE CATALOG and USE SCHEMA |
| Source freshness             | Query `INFORMATION_SCHEMA.TABLES` for `last_altered` | USE CATALOG                |
| Get view definition          | Query `SYSTEM.INFORMATION_SCHEMA.VIEWS`              | Access to system catalog   |
| Create schemas               | `CREATE SCHEMA IF NOT EXISTS`                        | CREATE SCHEMA on catalog   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Python models[​](#python-models "Direct link to Python models")

Optional permissions for environments using Python models

| Permission           | Object                 | Purpose                     |
| -------------------- | ---------------------- | --------------------------- |
| Workspace API access | `/api/2.0/workspace/*` | Create notebook directories |
| Notebook import      | Workspace              | Import Python notebooks     |
| Job execution        | Cluster or warehouse   | Run Python models           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Connection fields[​](#connection-fields "Direct link to Connection fields")

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

[![Configuring a Databricks connection using the dbt-databricks adapter](/img/docs/dbt-platform/platform-configuring-dbt-platform/dbt-databricks.png?v=2 "Configuring a Databricks connection using the dbt-databricks adapter")](#)Configuring a Databricks connection using the dbt-databricks adapter

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
