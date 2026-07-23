# Connect BigQuery to Fusion [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

You can configure the BigQuery adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type.

The BigQuery adapter for Fusion supports the following [authentication methods](#supported-authentication-types):

* Service account (JSON file)
* gcloud OAuth
* Workload Identity Federation (Microsoft Entra)

## Warehouse permissions[​](#warehouse-permissions "Direct link to Warehouse permissions")

The Google Cloud identity (service account or user) that dbt Fusion engine uses must have IAM permissions to run jobs, read and write table data, and read metadata Fusion uses for introspection and source freshness.

### Required Google Cloud objects[​](#required-google-cloud-objects "Direct link to Required Google Cloud objects")

Before connecting, these objects must exist or be accessible:

| Object                      | BigQuery term | Purpose                                             |
| --------------------------- | ------------- | --------------------------------------------------- |
| **Project**                 | GCP project   | Your Google Cloud project ID                        |
| **Dataset**                 | Dataset       | Target dataset (equivalent to a schema)             |
| **Service account or user** | IAM identity  | Identity for authentication                         |
| **Location or region**      | Location      | Data location (for example, `US`, `EU`, `us-east1`) |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### IAM permissions[​](#iam-permissions "Direct link to IAM permissions")

The following permissions are required for fundamental dbt features::

| Permission                   | Purpose                        |
| ---------------------------- | ------------------------------ |
| `bigquery.datasets.get`      | Access dataset metadata        |
| `bigquery.tables.create`     | Create tables                  |
| `bigquery.tables.delete`     | Drop or replace tables         |
| `bigquery.tables.get`        | Read table metadata            |
| `bigquery.tables.getData`    | Read table data                |
| `bigquery.tables.list`       | List tables in dataset         |
| `bigquery.tables.update`     | Update table schema            |
| `bigquery.tables.updateData` | Insert, update, or delete rows |
| `bigquery.jobs.create`       | Run queries                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The following are optional permissions for additional dbt features:

| Permission                 | When required                    |
| -------------------------- | -------------------------------- |
| `bigquery.datasets.create` | Auto-create datasets             |
| `bigquery.routines.create` | Create UDFs or stored procedures |
| `bigquery.jobs.get`        | Monitor job status               |
| `storage.objects.create`   | Python models (GCS staging)      |
| `dataproc.*`               | Python models on Dataproc        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Predefined IAM roles[​](#predefined-iam-roles "Direct link to Predefined IAM roles")

The following roles represent the typical starting point for dbt access:

| Role                        | Description                       | Use case                |
| --------------------------- | --------------------------------- | ----------------------- |
| `roles/bigquery.dataEditor` | Read and write tables in datasets | Standard dbt operations |
| `roles/bigquery.user`       | Run jobs, create datasets         | Job execution           |
| `roles/bigquery.jobUser`    | Run jobs only                     | Minimal query execution |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For Storage Read API access with Fusion, also grant **BigQuery Read Session User** (`roles/bigquery.readSessionUser`) on the project, as noted in [Connect BigQuery](https://docs.getdbt.com/docs/platform/connect-data-platform/connect-bigquery.md#required-permissions).

### Metadata operations[​](#metadata-operations "Direct link to Metadata operations")

The following are required for fundamental dbt features:

| Query type       | SQL or API used             | Required permission        |
| ---------------- | --------------------------- | -------------------------- |
| Get table schema | `get_table_schema` API      | `bigquery.tables.get`      |
| List relations   | Query against dataset       | `bigquery.tables.list`     |
| Source freshness | Query `__TABLES__` metadata | `bigquery.tables.get`      |
| Get table stats  | Query `INFORMATION_SCHEMA`  | `bigquery.tables.get`      |
| Create dataset   | `CREATE SCHEMA`             | `bigquery.datasets.create` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### INFORMATION\_SCHEMA and metadata views[​](#information_schema-and-metadata-views "Direct link to INFORMATION_SCHEMA and metadata views")

Fusion queries these BigQuery system views:

| View                            | Purpose                  | Scope             |
| ------------------------------- | ------------------------ | ----------------- |
| `INFORMATION_SCHEMA.TABLES`     | List tables              | Dataset or region |
| `INFORMATION_SCHEMA.COLUMNS`    | Column metadata          | Dataset or region |
| `INFORMATION_SCHEMA.VIEWS`      | View definitions         | Dataset or region |
| `INFORMATION_SCHEMA.PARTITIONS` | Partition information    | Dataset only      |
| `__TABLES__` (deprecated)       | Table modification times | Dataset           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### BigQuery DataFrames (optional)[​](#bigquery-dataframes "Direct link to BigQuery DataFrames (optional)")

For BigQuery DataFrames workflows, users typically need additional roles such as `BigQuery Job User`, `BigQuery Read Session User`, `Notebook Runtime User`, `Code Creator`, and `colabEnterpriseUser`. See your Google Cloud admin for exact role names in your organization.

## Configure Fusion[​](#configure-fusion "Direct link to Configure Fusion")

Executing `dbt init` in your CLI will prompt for the following fields:

* **Project ID:** The GCP BigQuery project ID
* **Dataset:** The schema name
* **Location:** The location for your GCP environment (for example, us-east1)

Alternatively, you can manually create the `profiles.yml` file and configure the fields. See examples in [authentication](#supported-authentication-types) section for formatting. If there is an existing `profiles.yml` file, you have the option to retain the existing fields or overwrite them.

Next, select your authentication method. Follow the on-screen prompts to provide the required information.

## Supported authentication types[​](#supported-authentication-types "Direct link to Supported authentication types")

* Service account (JSON file)
* gcloud OAuth
* Workload Identity Federation (Microsoft Entra)

Selecting the **Service account (JSON file)** authentication type will prompt you for the path to your JSON file. You can also manually define the path in your `profiles.yml` file.

#### Example service account JSON file configuration[​](#example-service-account-json-file-configuration "Direct link to Example service account JSON file configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: bigquery
      threads: 16
      database: ABC123
      schema: JAFFLE_SHOP
      method: service-account
      keyfile: /Users/youruser/Downloads/CustomRoleDefinition.json
      location: us-east1
      dataproc_batch: null
```

Prior to selecting this authentication method, you must first configure local OAuth for gcloud:

#### Local OAuth gcloud setup[​](#local-oauth-gcloud-setup "Direct link to Local OAuth gcloud setup")

1. Make sure the `gcloud` command is [installed on your computer](https://cloud.google.com/sdk/downloads).
2. Activate the application-default account with:

```shell
gcloud auth application-default login \           
  --scopes=https://www.googleapis.com/auth/bigquery,\
https://www.googleapis.com/auth/drive.readonly,\
https://www.googleapis.com/auth/iam.test,\
https://www.googleapis.com/auth/cloud-platform

# This command uses the `--scopes` flag to request access to Google Sheets. This makes it possible to transform data in Google Sheets using dbt. If your dbt project does not transform data in Google Sheets, then you may omit the `--scopes` flag.
```

A browser window should open, and you should be prompted to log into your Google account. Once you've done that, dbt will use your OAuth'd credentials to connect to BigQuery.

#### Example gcloud configuration[​](#example-gcloud-configuration "Direct link to Example gcloud configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: bigquery
      threads: 16
      database: ABC123
      schema: JAFFLE_SHOP
      method: oauth
      location: us-east1
      dataproc_batch: null
```

Workload Identity Federation (WIF) lets you authenticate to BigQuery using credentials issued by an external OAuth identity provider — currently Microsoft Entra — without managing a Google service account key file.

Fusion exchanges an Entra-issued token for short-lived Google credentials through a Google Cloud [workload identity pool](https://docs.cloud.google.com/iam/docs/workload-identity-federation#pools).

Before selecting this method, an administrator must configure a workload identity pool and provider in GCP that trusts your Entra tenant. You'll need the resulting workload pool provider path.

#### Configuration fields[​](#configuration-fields "Direct link to Configuration fields")

Here are the required and optional fields for the WIF config in your `profiles.yml` file:

| Field                               | Required | Description                                                                                                                                                   |
| ----------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `method`                            | Yes      | Set to `external-oauth-wif`.                                                                                                                                  |
| `workload_pool_provider_path`       | Yes      | The full resource path of the GCP workload identity pool provider that trusts your Entra tenant.                                                              |
| `token_endpoint.type`               | Yes      | The external identity provider type. Only `entra` is currently supported.                                                                                     |
| `token_endpoint.request_url`        | Yes      | The Entra OAuth token endpoint.                                                                                                                               |
| `token_endpoint.request_data`       | Yes      | The URL-encoded request body used to fetch the Entra token (for example `grant_type`, `client_id`, `client_secret`, and `scope`).                             |
| `service_account_impersonation_url` | No       | The `generateAccessToken` URL of a service account to impersonate after federation, if you want BigQuery access scoped to that service account's permissions. |

#### WIF config example in `profiles.yml`[​](#wif-config-example-in-profilesyml "Direct link to wif-config-example-in-profilesyml")

The following code snippet is an example of a WIF config in your `profiles.yml`. Make sure to replace the values with your own:

profiles.yml

```yaml
default:
  target: dev
  outputs:
    dev:
      type: bigquery
      threads: 16
      database: ABC123
      schema: JAFFLE_SHOP
      method: external-oauth-wif
      location: us-east1
      workload_pool_provider_path: //iam.googleapis.com/projects/123/locations/global/workloadIdentityPools/my-pool/providers/my-provider
      token_endpoint:
        type: entra
        request_url: https://login.microsoftonline.com/TENANT_ID/oauth2/v2.0/token
        request_data: "grant_type=client_credentials&client_id={{ env_var('ENTRA_CLIENT_ID') }}&client_secret={{ env_var('ENTRA_CLIENT_SECRET') }}&scope=SCOPE/.default"
        
      # Optional: include only if your workload identity pool doesn't have direct resource access and needs to impersonate a service account.
      # service_account_impersonation_url: https://iamcredentials.googleapis.com/v1/projects/-/serviceAccounts/sa@project.iam.gserviceaccount.com:generateAccessToken
```

## More information[​](#more-information "Direct link to More information")

Find BigQuery-specific configuration information in the [BigQuery adapter reference guide](https://docs.getdbt.com/reference/resource-configs/bigquery-configs.md).
