# Connect Databricks to Fusion [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local developmentⓘ

You can configure the Databricks adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type.

The Databricks adapter for Fusion supports the following [authentication methods](#supported-authentication-types):

* Personal access token (for individual users)
* Service Principal token (for service users)
* OAuth

## Warehouse permissions for Fusion[​](#warehouse-permissions-for-fusion "Direct link to Warehouse permissions for Fusion")

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

## Databricks configuration details[​](#databricks-configuration-details "Direct link to Databricks configuration details")

The dbt Fusion engine `dbt-databricks` adapter is the only supported connection method for Databricks.

`dbt-databricks` can connect to Databricks SQL Warehouses. These warehouses are the recommended way to get started with Databricks.

Refer to the [Databricks docs](https://docs.databricks.com/dev-tools/dbt.html#) for more info on how to obtain the credentials for configuring your profile.

## Configure Fusion[​](#configure-fusion "Direct link to Configure Fusion")

Executing `dbt init` in your CLI will prompt for the following fields:

* **Host:** Databricks instance hostname (excluding the `http` or `https` prefix)
* **HTTP Path:** Path to your SQL server or cluster
* **Schema:** The development/staging/deployment schema for the project
* **Catalog (Optional):** The Databricks catalog containing your schemas and tables

Alternatively, you can manually create the `profiles.yml` file and configure the fields. See examples in [authentication](#supported-authentication-types) section for formatting. If there is an existing `profiles.yml` file, you are given the option to retain the existing fields or overwrite them.

Next, select your authentication method. Follow the on-screen prompts to provide the required information.

## Supported authentication types[​](#supported-authentication-types "Direct link to Supported authentication types")

* Personal access token
* Service Principal token
* OAuth (Recommended)

Enter your personal access token (PAT) for the Databricks environment. For more information about obtaining a PAT, refer to the [Databricks documentation](https://docs.databricks.com/aws/en/dev-tools/auth/pat). This is considered a legacy feature by Databricks and OAuth is recommended over PATs.

#### Example personal access token configuration[​](#example-personal-access-token-configuration "Direct link to Example personal access token configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: databricks
      database: TRANSFORMING
      schema: JANE_SMITH
      host: YOUR.HOST.COM
      http_path: YOUR/PATH/HERE
      token: ABC123
      auth_type: databricks_cli
      threads: 16
```

Enter your Service Principal token for the Databricks environment. For more information about obtaining a Service Principal token, refer to the [Databricks documentation](https://docs.databricks.com/aws/en/admin/users-groups/service-principals).

#### Example Service Principal token configuration[​](#example-service-principal-token-configuration "Direct link to Example Service Principal token configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: databricks
      database: TRANSFORMING
      schema: JANE_SMITH
      host: YOUR.HOST.COM
      http_path: YOUR/PATH/HERE
      token: ABC123
      auth_type: databricks_cli
      threads: 16
```

Selecting the OAuth option will create a connection to your Databricks environment and open a web browser so you can complete the authentication. Users will be prompted to re-authenticate with each new dbt session they initiate.

#### Example OAuth configuration[​](#example-oauth-configuration "Direct link to Example OAuth configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: databricks
      database: TRANSFORMING
      schema: JANE_SMITH
      host: YOUR.HOST.COM
      http_path: YOUR/PATH/HERE
      auth_type: oauth
      threads: 16
```

## More information[​](#more-information "Direct link to More information")

Find Databricks-specific configuration information in the [Databricks adapter reference guide](https://docs.getdbt.com/reference/resource-configs/databricks-configs.md).
