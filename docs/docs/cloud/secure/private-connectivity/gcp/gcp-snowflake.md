# Configuring Snowflake Private Service Connect [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a GCP Snowflake Private Service Connect (PSC) endpoint in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Snowflake<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

warning

GCP Internal Stage PSC connections are not currently supported.

## Configure GCP Private Service Connect[​](#configure-gcp-private-service-connect "Direct link to Configure GCP Private Service Connect")

The dbt Labs GCP project has been pre-authorized for connections to Snowflake accounts.

To configure Snowflake instances hosted on GCP for [Private Service Connect](https://cloud.google.com/vpc/docs/private-service-connect):

1. Run the Snowflake system function [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config.html) and copy the output.

2. Add the required information to the following template and submit your request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

```text
Subject: New Multi-Tenant GCP PSC Request
- Type: Snowflake
- SYSTEM$GET_PRIVATELINK_CONFIG output:
- *Use privatelink-account-url or regionless-privatelink-account-url?: 
- dbt GCP multi-tenant environment:
```

*\*By default, dbt will be configured to use `privatelink-account-url` from the provided [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config.html) as the PrivateLink endpoint. Upon request, `regionless-privatelink-account-url` can be used instead.*

<!-- -->

dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once dbt Support completes the configuration, you can start creating new connections using PrivateLink.

1. Navigate to **Settings** → **Create new project** → select **Snowflake**.
2. You will see two radio buttons: **Public** and **Private**. Select **Private**.
3. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
4. Configure the remaining data platform details.
5. Test your connection and save it.

## Configuring network policies[​](#configuring-network-policies "Direct link to Configuring network policies")

If your organization uses [Snowflake Network Policies](https://docs.snowflake.com/en/user-guide/network-policies) to restrict access to your Snowflake account, you need to add a network rule for dbt.

Request the **PSC connection ID** from [dbt Support](mailto:support@getdbt.com) to use in a network rule. Snowflake supports [`GCPPSCID` as a network rule identifier type](https://docs.snowflake.com/en/sql-reference/sql/create-network-rule), and this is the recommended approach. A PSC connection ID uniquely identifies your organization's connection endpoint, whereas IP-based rules rely on CIDR ranges that may be shared across multiple dbt customers.

### Using the UI[​](#using-the-ui "Direct link to Using the UI")

Open the Snowflake UI and take the following steps:

1. Go to the **Security** tab.
2. Click on **Network Rules**.
3. Click on **Add Rule**.
4. Give the rule a name.
5. Select a database and schema where the rule will be stored. These selections are for permission settings and organizational purposes; they do not affect the rule itself.
6. Set the type to `GCPPSCID` and the mode to `Ingress`.
7. Enter the PSC connection ID provided by dbt Support into the identifier box and press **Enter**.
8. Click **Create Network Rule**.
9. In the **Network Policy** tab, edit the policy you want to add the rule to. This could be your account-level policy or a policy specific to the users connecting from dbt.
10. Add the new rule to the allowed list and click **Update Network Policy**.

### Using SQL[​](#using-sql "Direct link to Using SQL")

For quick and automated setup of network rules via SQL in Snowflake, the following commands allow you to create and configure access rules for dbt. These SQL examples demonstrate how to add a network rule and update your network policy accordingly.

1. Create a new network rule with the following SQL:

```sql

CREATE NETWORK RULE allow_dbt_cloud_access
  MODE = INGRESS
  TYPE = GCPPSCID
  VALUE_LIST = ('<PSC_CONNECTION_ID>'); -- Replace with the PSC connection ID from dbt Support
```

2. Add the rule to a network policy with the following SQL:

```sql

ALTER NETWORK POLICY <network_policy_name>
  ADD ALLOWED_NETWORK_RULE_LIST =('allow_dbt_cloud_access');
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
