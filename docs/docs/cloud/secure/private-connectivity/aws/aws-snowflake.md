# Configuring Snowflake PrivateLink [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of an AWS-hosted Snowflake PrivateLink endpoint in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Snowflake<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

<!-- -->

Snowflake OAuth with PrivateLink

Users connecting to Snowflake using [Snowflake OAuth](https://docs.getdbt.com/docs/cloud/manage-access/set-up-snowflake-oauth.md) over an AWS PrivateLink connection from dbt will also require access to a PrivateLink endpoint from their local workstation. Where possible, use [Snowflake External OAuth](https://docs.getdbt.com/docs/cloud/manage-access/snowflake-external-oauth.md) instead to bypass this limitation.

From the [Snowflake](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview#label-sso-private-connectivity) docs:

> Currently, for any given Snowflake account, SSO works with only one account URL at a time: either the public account URL or the URL associated with the private connectivity service

## Configure AWS PrivateLink[​](#configure-aws-privatelink "Direct link to Configure AWS PrivateLink")

To configure Snowflake instances hosted on AWS for [PrivateLink](https://aws.amazon.com/privatelink):

1. Open a support case with Snowflake to allow access from the dbt AWS account.

   <!-- -->

   * Snowflake prefers that the account owner opens the support case directly rather than dbt Labs acting on their behalf. For more information, refer to [Snowflake's knowledge base article](https://community.snowflake.com/s/article/HowtosetupPrivatelinktoSnowflakefromCloudServiceVendors).
   * Provide them with your dbt account ID along with any other information requested in the article.
     <!-- -->
     * **AWS account ID**: `346425330055` — *Note: This account ID only applies to AWS dbt multi-tenant environments. For AWS Virtual Private/Single-Tenant account IDs, contact [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support).*
   * You need `ACCOUNTADMIN` access to the Snowflake instance to submit a support request.

[![Open snowflake case](/img/docs/dbt-cloud/snowflakeprivatelink1.png?v=2 "Open snowflake case")](#)Open snowflake case

2. After Snowflake has granted the requested access, run the Snowflake system function [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config.html) and copy the output.

3. Add the required information to the following template and submit your request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

```text
Subject: New Multi-Tenant (Azure or AWS) PrivateLink Request
- Type: Snowflake
- SYSTEM$GET_PRIVATELINK_CONFIG output:
- *Use privatelink-account-url or regionless-privatelink-account-url?:
- **Create Internal Stage PrivateLink endpoint? (Y/N): 
- dbt AWS multi-tenant environment (US, EMEA, AU):
```

*\*By default, dbt will be configured to use `privatelink-account-url` from the provided [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config.html) as the PrivateLink endpoint. Upon request, `regionless-privatelink-account-url` can be used instead.*

*\*\* Internal Stage PrivateLink must be [enabled on the Snowflake account](https://docs.snowflake.com/en/user-guide/private-internal-stages-aws#prerequisites) to use this feature*

<!-- -->

dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once dbt Support completes the configuration, you can start creating new connections using PrivateLink.

1. Navigate to **Settings** → **Create new project** → select **Snowflake**.
2. You will see two radio buttons: **Public** and **Private**. Select **Private**.
3. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
4. Configure the remaining data platform details.
5. Test your connection and save it.

## Configuring internal stage PrivateLink in dbt[​](#configuring-internal-stage-privatelink-in- "Direct link to configuring-internal-stage-privatelink-in-")

If an Internal Stage PrivateLink endpoint has been provisioned, your dbt environments must be configured to use this endpoint instead of the account default set in Snowflake.

1. Obtain the Internal Stage PrivateLink endpoint DNS from dbt Support. For example, `*.vpce-012345678abcdefgh-4321dcba.s3.us-west-2.vpce.amazonaws.com`.
2. In the appropriate dbt project, navigate to **Orchestration** → **Environments**.
3. In any environment that should use the dbt Internal Stage PrivateLink endpoint, set an **Extended Attribute** similar to the following:

```text
s3_stage_vpce_dns_name: '*.vpce-012345678abcdefgh-4321dcba.s3.us-west-2.vpce.amazonaws.com'
```

4. Save the changes.

[![Internal Stage DNS](/img/docs/dbt-cloud/snowflake-internal-stage-dns.png?v=2 "Internal Stage DNS")](#)Internal Stage DNS

## Configuring network policies[​](#configuring-network-policies "Direct link to Configuring network policies")

If your organization uses [Snowflake Network Policies](https://docs.snowflake.com/en/user-guide/network-policies) to restrict access to your Snowflake account, you need to add a network rule for dbt.

You can request the VPCE IDs from [dbt Support](mailto:support@getdbt.com), that you can use to create a network policy. If creating an endpoint for Internal Stage, the VPCE ID will be different from the VPCE ID of the main service endpoint.

Network Policy for Snowflake Internal Stage PrivateLink

For guidance on protecting both the Snowflake service and Internal Stage consult the Snowflake [network policies](https://docs.snowflake.com/en/user-guide/network-policies#strategies-for-protecting-both-service-and-internal-stage) and [network rules](https://docs.snowflake.com/en/user-guide/network-rules#incoming-requests) docs.

### Using the UI[​](#using-the-ui "Direct link to Using the UI")

Open the Snowflake UI and take the following steps:

1. Go to the **Security** tab.
2. Click on **Network Rules**.
3. Click on **Add Rule**.
4. Give the rule a name.
5. Select a database and schema where the rule will be stored. These selections are for permission settings and organizational purposes; they do not affect the rule itself.
6. Set the type to `AWS VPCE ID` and the mode to `Ingress`.
7. Type the VPCE ID provided by dbt Support into the identifier box and press **Enter**.
8. Click **Create Network Rule**.

[![Create Network Rule](/img/docs/dbt-cloud/snowflakeprivatelink2.png?v=2 "Create Network Rule")](#)Create Network Rule

9. In the **Network Policy** tab, edit the policy you want to add the rule to. This could be your account-level policy or a policy specific to the users connecting from dbt.

10. Add the new rule to the allowed list and click **Update Network Policy**.

[![Update Network Policy](/img/docs/dbt-cloud/snowflakeprivatelink3.png?v=2 "Update Network Policy")](#)Update Network Policy

### Using SQL[​](#using-sql "Direct link to Using SQL")

For quick and automated setup of network rules via SQL in Snowflake, the following commands allow you to create and configure access rules for dbt. These SQL examples demonstrate how to add a network rule and update your network policy accordingly.

1. Create a new network rule with the following SQL:

```sql

CREATE NETWORK RULE allow_dbt_cloud_access
  MODE = INGRESS
  TYPE = AWSVPCEID
  VALUE_LIST = ('<VPCE_ID>'); -- Replace '<VPCE_ID>' with the actual ID provided
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
