# Redshift setup [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

You can configure the Redshift adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type.

The Redshift adapter for Fusion supports the following [authentication methods](#supported-authentication-types):

* Password
* IAM profile

## Configure Fusion[​](#configure-fusion "Direct link to Configure Fusion")

Executing `dbt init` in your CLI will prompt for the following fields:

* **Host:** The hostname of your Redshift cluster
* **User:** Username of the account that will be connecting to the database
* **Database:** The database name
* **Schema:** The schema name
* **Port (default: 5439):** Port for your Redshift environment

Alternatively, you can manually create the `profiles.yml` file and configure the fields. See examples in [authentication](#supported-authentication-types) section for formatting. If there is an existing `profiles.yml` file, you are given the option to retain the existing fields or overwrite them.

Next, select your authentication method. Follow the on-screen prompts to provide the required information.

## Supported authentication types[​](#supported-authentication-types "Direct link to Supported authentication types")

* Password
* IAM profile

Use your Redshift user's password to authenticate. You can also manually enter it in plain text into the `profiles.yml` file configuration.

#### Example password configuration[​](#example-password-configuration "Direct link to Example password configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: redshift
      port: 5439
      database: JAFFLE_SHOP
      schema: JAFFLE_TEST
      ra3_node: true
      method: database
      host: ABC123.COM
      user: JANE.SMITH@YOURCOMPANY.COM
      password: ABC123
      threads: 16
```

Specify the IAM profile to use to connect your Fusion sessions. You will need to provide the following information:

* **IAM Profile:** The profile name
* **Cluster ID:** The unique identifier for your AWS cluster
* **Region:** Your AWS region (for example, us-east-1)
* **Use RA3 node type (y/n):** Use high performance AWS RA3 node

#### Example password configuration[​](#example-password-configuration-1 "Direct link to Example password configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: redshift
      port: 5439
      database: JAFFLE_SHOP
      schema: JAFFLE_TEST
      ra3_node: false
      method: iam
      host: YOURHOSTNAME.COM
      user: JANE.SMITH@YOURCOMPANY.COM
      iam_profile: YOUR_PROFILE_NAME
      cluster_id: ABC123
      region: us-east-1
      threads: 16
```

## More information[​](#more-information "Direct link to More information")

Find Redshift-specific configuration information in the [Redshift adapter reference guide](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
