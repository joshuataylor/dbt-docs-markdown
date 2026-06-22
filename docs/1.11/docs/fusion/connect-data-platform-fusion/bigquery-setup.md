# BigQuery setup [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

You can configure the BigQuery adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type.

The BigQuery adapter for Fusion supports the following [authentication methods](#supported-authentication-types):

* Service account (JSON file)
* gcloud OAuth

## BigQuery permssions[​](#bigquery-permssions "Direct link to BigQuery permssions")

dbt user accounts need the following permissions to read from and create tables and views in a BigQuery project:

* BigQuery Data Editor
* BigQuery User
* BigQuery Read Session User (New in Fusion. For Storage Read API access)

For BigQuery DataFrames, users need these additional permissions:

* BigQuery Job User
* BigQuery Read Session User
* Notebook Runtime User
* Code Creator
* colabEnterpriseUser

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
      keyfile: /Users/mshaver/Downloads/CustomRoleDefinition.json
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

## More information[​](#more-information "Direct link to More information")

Find BigQuery-specific configuration information in the [BigQuery adapter reference guide](https://docs.getdbt.com/reference/resource-configs/bigquery-configs.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
