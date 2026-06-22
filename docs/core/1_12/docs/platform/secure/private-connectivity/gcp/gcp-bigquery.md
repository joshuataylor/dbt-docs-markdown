# Configuring BigQuery Private Service Connect [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

Available to certain Enterprise tiers

This feature is available on the following dbt Enterprise tiers:

* Enterprise+
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a GCP BigQuery [Private Service Connect](https://cloud.google.com/vpc/docs/private-service-connect) (PSC) endpoint in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->BigQuery<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Enabling dbt for GCP Private Service Connect[​](#enabling-dbt-for-gcp-private-service-connect "Direct link to Enabling dbt for GCP Private Service Connect")

To enable dbt to privately connect to your BigQuery project via PSC, the regional PSC endpoint needs be enabled for your dbt account. Using the following template, submit a request to [dbt Support](mailto:support@getdbt.com):

 Support request email template

```text
Subject: New Multi-Tenant GCP PSC Request

- Type: BigQuery
- dbt platform account URL:
- BigQuery project region:
- dbt GCP multi-tenant environment:
```

<!-- -->

dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

## (Optional) Generate BigQuery credentials[​](#optional-generate-bigquery-credentials "Direct link to (Optional) Generate BigQuery credentials")

You may already have credentials set up for your datasets. If not, you can follow the steps in our [BigQuery quickstart guide](https://docs.getdbt.com/guides/bigquery.md?step=4) to generate credentials.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once dbt Support completes the configuration, you can start creating new connections using PSC:

1. Navigate to **Account Settings** → **Projects** → **Create new project**.
2. Under the **Configure your development environment step** section, select **Add new connection**.
3. You'll be directed to the **Add new connection** page, select **BigQuery**.
4. Under the **Settings** section, select **PrivateLink Endpoint**.
5. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
6. Configure the remaining data platform details.
7. Test your connection and save it.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
