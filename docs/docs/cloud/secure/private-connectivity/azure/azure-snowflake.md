# Configuring Snowflake and Azure Private Link

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of an Azure-hosted Snowflake Private Link endpoint in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Snowflake<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

Snowflake OAuth with Private Link

Users connecting to Snowflake using [Snowflake OAuth](https://docs.getdbt.com/docs/cloud/manage-access/set-up-snowflake-oauth.md) over an Azure Private Link connection from dbt also require access to a Private Link endpoint from their local workstation. Where possible, use [Snowflake External OAuth](https://docs.getdbt.com/docs/cloud/manage-access/snowflake-external-oauth.md) instead to bypass this limitation.

Snowflake docs:

> Currently, for any given Snowflake account, SSO works with only one account URL at a time: either the public account URL or the URL associated with the private connectivity service

* [Snowflake SSO with Private Connectivity](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview#label-sso-private-connectivity)

## Configure Azure Private Link[​](#configure-azure-private-link "Direct link to Configure Azure Private Link")

To configure Snowflake instances hosted on Azure for [Private Link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview):

1. In your Snowflake account, run the following SQL statements and copy the output:

```sql

USE ROLE ACCOUNTADMIN;
SELECT SYSTEM$GET_PRIVATELINK_CONFIG();
```

2. Add the required information to the following template and submit your request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

```text
Subject: New Multi-Tenant Azure PrivateLink Request
- Type: Snowflake
- The output from SYSTEM$GET_PRIVATELINK_CONFIG:
  - Include the privatelink-pls-id
  - Enable Internal Stage Private Link? Y/N (If Y, output must include `privatelink-internal-stage`)
- dbt Azure multi-tenant environment (EMEA):
```

3. dbt Support will provide the `private endpoint resource_id` of our `private_endpoint` and the `CIDR` range for you to complete the [PrivateLink configuration](https://community.snowflake.com/s/article/HowtosetupPrivatelinktoSnowflakefromCloudServiceVendors) by contacting the Snowflake Support team.

4. (Optional) If enabling an [Azure private endpoint for an Internal Stage](https://docs.snowflake.com/en/user-guide/private-internal-stages-azure), it will also provide the `resource_id` for the Internal Stage endpoint.

As the Snowflake administrator, call the `SYSTEM$AUTHORIZE_STAGE_PRIVATELINK_ACCESS` function using the resource ID value as the function argument. This authorizes access to the Snowflake internal stage through the private endpoint.

```sql

USE ROLE ACCOUNTADMIN;

-- Azure Private Link
SELECT SYSTEM$AUTHORIZE_STAGE_PRIVATELINK_ACCESS ( 'AZURE_PRIVATE_ENDPOINT_RESOURCE_ID' );
```

## Configuring network policies[​](#configuring-network-policies "Direct link to Configuring network policies")

If your organization uses [Snowflake Network Policies](https://docs.snowflake.com/en/user-guide/network-policies) to restrict access to your Snowflake account, you need to add a network rule for dbt.

### Find the endpoint Azure Link ID[​](#find-the-endpoint-azure-link-id "Direct link to Find the endpoint Azure Link ID")

Snowflake allows you to find the Azure Link ID of configured endpoints by running the `SYSTEM$GET_PRIVATELINK_AUTHORIZED_ENDPOINTS` command. Use the following to isolate the Link ID value and the associated endpoint resource name:

```sql

select
  value:linkIdentifier, REGEXP_SUBSTR(value: endpointId, '([^\/]+$)')
from
  table(
    flatten(
      input => parse_json(system$get_privatelink_authorized_endpoints())
    )
  );
```

### Using the UI[​](#using-the-ui "Direct link to Using the UI")

Open the Snowflake UI and take the following steps:

1. Go to the **Security** tab.
2. Click on **Network Rules**.
3. Click on **+ Network Rule**.
4. Give the rule a name.
5. Select a database and schema where the rule will be stored. These selections are for permission settings and organizational purposes; they do not affect the rule itself.
6. Set the type to `Azure Link ID` and the mode to `Ingress`.
7. In the identifier box, type the Azure Link ID obtained in the previous section and press **Enter**.
8. Click **Create Network Rule**.

[![Create Network Rule](/img/docs/dbt-cloud/snowflakeprivatelink2.png?v=2 "Create Network Rule")](#)Create Network Rule

9. In the **Network Policy** tab, edit the policy to which you want to add the rule. This could be your account-level policy or one specific to the users connecting from dbt.

10. Add the new rule to the allowed list and click **Update Network Policy**.

[![Update Network Policy](/img/docs/dbt-cloud/snowflakeprivatelink3.png?v=2 "Update Network Policy")](#)Update Network Policy

### Using SQL[​](#using-sql "Direct link to Using SQL")

For quick and automated setup of network rules via SQL in Snowflake, the following commands allow you to create and configure access rules for dbt. These SQL examples demonstrate how to add a network rule and update your network policy accordingly.

1. Create a new network rule with the following SQL:

```sql

CREATE NETWORK RULE allow_dbt_cloud_access
  MODE = INGRESS
  TYPE = AZURELINKID
  VALUE_LIST = ('<Azure Link ID>'); -- Replace '<Azure Link ID>' with the actual ID obtained above
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
