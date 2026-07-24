# Configuring Snowflake PrivateLink

dbt platform | Enterprise+ⓘ

<!-- -->

Available to certain Enterprise tiers

This feature is available on the following dbt Enterprise tiers:

* Enterprise+
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of an AWS-hosted Snowflake PrivateLink endpoint in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Snowflake<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

<!-- -->

Snowflake OAuth with PrivateLink

Users connecting to Snowflake using [Snowflake OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-snowflake-oauth.md) over an AWS PrivateLink connection from dbt will also require access to a PrivateLink endpoint from their local workstation. Where possible, use [Snowflake External OAuth](https://docs.getdbt.com/docs/platform/manage-access/snowflake-external-oauth.md) instead to bypass this limitation.

From the [Snowflake](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview#label-sso-private-connectivity) docs:

> Currently, for any given Snowflake account, SSO works with only one account URL at a time: either the public account URL or the URL associated with the private connectivity service

## Configure AWS PrivateLink[​](#configure-aws-privatelink "Direct link to Configure AWS PrivateLink")

This section walks you through the setup of an AWS-hosted Snowflake PrivateLink endpoint in a dbt platform. You can set up in two ways:

* [Self-serve private endpoints](#self-serve-private-endpoints): Self-serve configuration of Snowflake PrivateLink endpoints directly in dbt platform user interface. Currently in beta.
* [Support-led setup](#support-led-setup): Requires contacting dbt Support to configure Snowflake PrivateLink endpoints. Non-self service configuration of Snowflake PrivateLink endpoints.

### Self-serve private endpoints [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#self-serve-private-endpoints- "Direct link to self-serve-private-endpoints-")

*Self-serve private endpoints are currently in beta for Snowflake on AWS, and available to all eligible customers. This feature isn't available for Azure or GCP. If you don't see **Private endpoints** in your account settings, use the [Support-led setup](#support-led-setup) instead.*

This section walks you through the process of requesting a new Snowflake PrivateLink endpoint in dbt platform.

##### Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [Account admin](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md?version=2.0#account-admin) or [Project creator](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md?version=2.0#project-creator) permission sets in dbt platform. Users with an IT license can also create private endpoints.
* [Snowflake `ACCOUNTADMIN` permissions](https://docs.snowflake.com/en/user-guide/security-access-control-overview#system-defined-roles).

##### Before you begin[​](#before-you-begin "Direct link to Before you begin")

Before requesting a private endpoint, allowlist dbt Labs' AWS account in Snowflake. This is a one-time setup per Snowflake account and is handled through a Snowflake support case, which can take time — complete it before starting the request flow.

1. Open a support case with Snowflake to allow access from the dbt AWS account.

   * Snowflake prefers that the account owner opens the support case directly rather than dbt Labs acting on their behalf. For more information, refer to [Snowflake's knowledge base article](https://community.snowflake.com/s/article/HowtosetupPrivatelinktoSnowflakefromCloudServiceVendors).
   * Provide your dbt account ID along with any other information requested in the article.
     <!-- -->
     * **AWS account ID**: `346425330055` — *Note: This account ID only applies to AWS dbt multi-tenant environments. For AWS Virtual Private/Single-Tenant account IDs, contact [dbt Support](mailto:support@getdbt.com).*

2. Wait for Snowflake to confirm access has been granted before proceeding.

   [![Open snowflake case](/img/docs/dbt-platform/snowflakeprivatelink1.png?v=2 "Open snowflake case")](#)Open snowflake case

#### Request a new private endpoint[​](#request-a-new-private-endpoint "Direct link to Request a new private endpoint")

After Snowflake confirms they've allowlisted dbt Labs' AWS account in Snowflake, you can request a new private endpoint. Follow these steps to do so:

1. In dbt platform, go to **Account settings → Private endpoints**.

2. In the **Private endpoints** table, review your existing endpoints. The table shows all private endpoints in your account (including non-Snowflake ones) with the following details:

   * **Name**
   * **Connection type** (for example, Snowflake)
   * **URL**
   * **Connectivity status** (for example, **Success** or **Unknown**)
   * **Connections** — the number of dbt platform connections using the endpoint

   You can search by **Name** or **URL**. The table lists all private endpoints in your account, but self-serve create, edit, and delete is only available for Snowflake on AWS at this time.

   [![Private endpoints table showing existing endpoints, connectivity status, and the Request new button](/img/docs/dbt-platform/private-endpoint-page.png?v=2 "Private endpoints table showing existing endpoints, connectivity status, and the Request new button")](#)Private endpoints table showing existing endpoints, connectivity status, and the Request new button

3. To request a new endpoint, click **Request new**.

4. Under **Provider type**, confirm **Snowflake** is selected. Currently other endpoint providers aren't supported, contact [dbt Support](mailto:support@getdbt.com) if you need to connect to a different service.

5. Copy the SQL command in the **SQL command snippet** section.

6. Go to Snowflake and run the SQL command snippet you copied from dbt platform: `SELECT SYSTEM$GET_PRIVATELINK_CONFIG();`

7. Copy the output from Snowflake and return to dbt platform to paste it into the **Snowflake output** field. If the output is correct, you'll see an inline **Output looks good** type message below the text box. If there's an error, review the message and make any updates as necessary.

8. Click **Submit request**.

   [![Endpoint request form showing Provider type, SQL command snippet, and Snowflake output fields](/img/docs/dbt-platform/private-endpoint-config.png?v=2 "Endpoint request form showing Provider type, SQL command snippet, and Snowflake output fields")](#)Endpoint request form showing Provider type, SQL command snippet, and Snowflake output fields

9. After submission, a confirmation popup appears (for example, **Endpoint request submitted**). From the popup, you can request another endpoint or return to **Private endpoints** to track request status.

10. Proceed to the **Connections** page and following the steps in the [Create connection in dbt](#create-connection-in-dbt) section to configure PrivateLink. Once you configure PrivateLink on the **Connections** page, you'll see the new endpoint appear under **Private endpoints → Associated connections**.

DNS propagation

If the connection test fails immediately after setup, this is expected — it doesn't mean something is wrong. DNS changes can take a few minutes to propagate. Wait a few minutes, then test again before contacting support.

 Edit or delete a private endpoint

If you don't see **Edit** or **Delete endpoint**, contact your account manager to enable private endpoint updates for your account.

**Edit an endpoint**

1. In the **Private endpoints** table, click the endpoint you want to update.
2. On the endpoint details page, click **Edit**.
3. Update **Name** and/or **Port** as needed.
4. Click **Save changes**.
5. In the **Save changes?** modal, click **Save changes** to apply your updates.

[![Private endpoint details page with the Edit button](/img/docs/dbt-platform/private-endpoint-details-edit.png?v=2 "Private endpoint details page with the Edit button")](#)Private endpoint details page with the Edit button

**Delete an endpoint**

An endpoint with associated connections can't be deleted. Remove those connections first.

1. In the **Private endpoints** table, click the endpoint you want to delete.
2. On the endpoint details page, click **Edit**.
3. Scroll to the bottom of the page and click **Delete endpoint**.
4. In the **Delete endpoint** modal, type `DELETE` to confirm, then click **Delete endpoint**.

[![Private endpoint details page in edit mode with the Delete endpoint button](/img/docs/dbt-platform/private-endpoint-details-delete.png?v=2 "Private endpoint details page in edit mode with the Delete endpoint button")](#)Private endpoint details page in edit mode with the Delete endpoint button

#### Duplicate endpoint requests[​](#duplicate-endpoint-requests "Direct link to Duplicate endpoint requests")

If you submit a request using a VPCE ID that matches an existing endpoint, dbt platform displays an **Endpoint already exists** popup with two options:

* **Reuse existing interface endpoint** (default, recommended) — Links the new private endpoint to an already-approved interface endpoint. Use this option when your VPCE is already approved to avoid duplicating infrastructure.
* **Create new interface endpoint** — Creates a new interface endpoint with its own network policy. Use this only if you need a distinct network policy configuration.

Select your preferred option and click **Confirm & Submit**.

[![Endpoint already exists popup with options to create a new interface endpoint or re-use an existing one](/img/docs/dbt-platform/endpoint-exists.png?v=2 "Endpoint already exists popup with options to create a new interface endpoint or re-use an existing one")](#)Endpoint already exists popup with options to create a new interface endpoint or re-use an existing one

#### Troubleshooting and errors[​](#troubleshooting-and-errors "Direct link to Troubleshooting and errors")

If an endpoint request fails, dbt platform displays error details that are safe to share externally.

If you see a failure state without clear next steps, collect the request details (endpoint name, creation time, status, and the Snowflake output you provided) and contact [dbt Support](mailto:support@getdbt.com).

### Support-led setup[​](#support-led-setup "Direct link to Support-led setup")

If **Private endpoints** is not available in your account settings, configure Snowflake PrivateLink by following these steps and submitting a request to dbt Support.

To configure Snowflake instances hosted on AWS for [PrivateLink](https://aws.amazon.com/privatelink):

1. Open a support case with Snowflake to allow access from the dbt AWS account.

   <!-- -->

   * Snowflake prefers that the account owner opens the support case directly rather than dbt Labs acting on their behalf. For more information, refer to [Snowflake's knowledge base article](https://community.snowflake.com/s/article/HowtosetupPrivatelinktoSnowflakefromCloudServiceVendors).
   * Provide them with your dbt account ID along with any other information requested in the article.
     <!-- -->
     * **AWS account ID**: `346425330055` — *Note: This account ID only applies to AWS dbt multi-tenant environments. For AWS Virtual Private/Single-Tenant account IDs, contact [dbt Support](mailto:support@getdbt.com).*
   * You need [Snowflake's `ACCOUNTADMIN` permissions](https://docs.snowflake.com/en/user-guide/security-access-control-overview#system-defined-roles).

[![Open snowflake case](/img/docs/dbt-platform/snowflakeprivatelink1.png?v=2 "Open snowflake case")](#)Open snowflake case

2. After Snowflake has granted the requested access, run the Snowflake system function [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config.html) and copy the output.

3. Add the required information to the following template and submit your request to [dbt Support](mailto:support@getdbt.com):

 Support request email template

```text
Subject: New Multi-Tenant (Azure or AWS) PrivateLink Request

- Type: Snowflake
- dbt platform account URL:
- SYSTEM$GET_PRIVATELINK_CONFIG output:
- *Use privatelink-account-url or regionless-privatelink-account-url?:
- **Create Internal Stage PrivateLink endpoint? (Y/N):
- dbt AWS multi-tenant environment (US, EMEA, AU, JP):
```

*\*By default, dbt will be configured to use `privatelink-account-url` from the provided [SYSTEM$GET\_PRIVATELINK\_CONFIG](https://docs.snowflake.com/en/sql-reference/functions/system_get_privatelink_config.html) as the PrivateLink endpoint. Upon request, `regionless-privatelink-account-url` can be used instead.*

*\*\* Internal Stage PrivateLink must be [enabled on the Snowflake account](https://docs.snowflake.com/en/user-guide/private-internal-stages-aws#prerequisites) to use this feature*

<!-- -->

dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once dbt Support completes the configuration, you can start creating new connections using PrivateLink.

1. Navigate to **Account Settings** → **Projects** → **Create new project**.
2. Under the **Configure your development environment step** section, select **Add new connection**.
3. You'll be directed to the **Add new connection** page, select **Snowflake**.
4. Under the **Settings** section, select **PrivateLink Endpoint**.
5. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
6. Configure the remaining data platform details.
7. Test your connection and save it.

## Configuring internal stage PrivateLink in dbt[​](#configuring-internal-stage-privatelink-in- "Direct link to configuring-internal-stage-privatelink-in-")

If an Internal Stage PrivateLink endpoint has been provisioned, your dbt environments must be configured to use this endpoint instead of the account default set in Snowflake.

1. Obtain the Internal Stage PrivateLink endpoint DNS from dbt Support. For example, `*.vpce-012345678abcdefgh-4321dcba.s3.us-west-2.vpce.amazonaws.com`.
2. In the appropriate dbt project, navigate to **Orchestration** → **Environments**.
3. In any environment that should use the dbt Internal Stage PrivateLink endpoint, set an **Extended Attribute** similar to the following:

```text
s3_stage_vpce_dns_name: '*.vpce-012345678abcdefgh-4321dcba.s3.us-west-2.vpce.amazonaws.com'
```

4. Save the changes.

[![Internal Stage DNS](/img/docs/dbt-platform/snowflake-internal-stage-dns.png?v=2 "Internal Stage DNS")](#)Internal Stage DNS

## Configuring network policies[​](#configuring-network-policies "Direct link to Configuring network policies")

If your organization uses [Snowflake Network Policies](https://docs.snowflake.com/en/user-guide/network-policies) to restrict access to your Snowflake account, you need to add a network rule for dbt. Snowflake supports two network rule types: VPCE ID-based (recommended) and IP/CIDR-based. The following steps use VPCE ID. If your organization requires an IP-based network policy instead, the CIDR range isn't available in the dbt platform UI. Please contact [dbt Support](mailto:support@getdbt.com) to get it.

You need a VPCE ID to create a network policy in Snowflake:

1. In dbt platform, go to **Account settings → Private endpoints**
2. Click the endpoint in the table, then locate its **Endpoint identifier** field on the endpoint details page.
3. If you configured PrivateLink through [Support-led setup](#support-led-setup), or **Private endpoints** is not available in your account settings, contact [dbt Support](mailto:support@getdbt.com) to obtain the VPCE ID.
4. If you're creating an endpoint for Internal Stage, the VPCE ID is different from the VPCE ID for the main service endpoint.

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
7. Enter the VPCE ID from the endpoint details page in dbt platform (or from dbt Support if you used Support-led setup) into the identifier box.
8. Click **Create Network Rule**.

[![Create Network Rule](/img/docs/dbt-platform/snowflakeprivatelink2.png?v=2 "Create Network Rule")](#)Create Network Rule

9. In the **Network Policy** tab, edit the policy you want to add the rule to. This could be your account-level policy or a policy specific to the users connecting from dbt.

10. Add the new rule to the allowed list and click **Update Network Policy**.

[![Update Network Policy](/img/docs/dbt-platform/snowflakeprivatelink3.png?v=2 "Update Network Policy")](#)Update Network Policy

### Using SQL[​](#using-sql "Direct link to Using SQL")

For quick and automated setup of network rules via SQL in Snowflake, the following commands allow you to create and configure access rules for dbt. These SQL examples demonstrate how to add a network rule and update your network policy accordingly.

1. Create a new network rule with the following SQL:

```sql

CREATE NETWORK RULE allow_dbt_cloud_access
  MODE = INGRESS
  TYPE = AWSVPCEID
  VALUE_LIST = ('<VPCE_ID>'); -- Replace '<VPCE_ID>' with the VPCE ID the actual value
```

2. Add the rule to a network policy with the following SQL:

```sql

ALTER NETWORK POLICY <network_policy_name>
  ADD ALLOWED_NETWORK_RULE_LIST =('allow_dbt_cloud_access');
```
