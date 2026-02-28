# Configuring Databricks PrivateLink [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a Databricks AWS PrivateLink endpoint in the dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Databricks<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Configure AWS PrivateLink[​](#configure-aws-privatelink "Direct link to Configure AWS PrivateLink")

1. Locate your [Databricks instance name](https://docs.databricks.com/en/workspace/workspace-details.html#workspace-instance-names-urls-and-ids).

   * Example: `cust-success.cloud.databricks.com`

2. Add the required information to the following template and submit your AWS PrivateLink request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

   ```text
   Subject: New AWS Multi-Tenant PrivateLink Request
   - Type: Databricks
   - Databricks instance name:
   - Databricks cluster AWS Region (for example, us-east-1, eu-west-2):
   - dbt AWS multi-tenant environment (US, EMEA, AU):
   ```

   dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

3. Once dbt Support notifies you that setup is complete, [register the VPC endpoint in Databricks](https://docs.databricks.com/administration-guide/cloud-configurations/aws/privatelink.html#step-3-register-privatelink-objects-and-attach-them-to-a-workspace) and attach it to the workspace:

   * [Register your VPC endpoint](https://docs.databricks.com/en/security/network/classic/vpc-endpoints.html) — Register the VPC endpoint using the VPC endpoint ID provided by dbt Support.
   * [Create a Private Access Settings object](https://docs.databricks.com/en/security/network/classic/private-access-settings.html) — Create a Private Access Settings (PAS) object with your desired public access settings, and setting Private Access Level to **Endpoint**. Choose the registered endpoint created in the previous step.
   * [Create or update your workspace](https://docs.databricks.com/en/security/network/classic/privatelink.html#step-3d-create-or-update-the-workspace-front-end-back-end-or-both) — Create a workspace, or update an existing workspace. Under **Advanced configurations → Private Link** choose the private access settings object created in the previous step.

   warning

   If using an existing Databricks workspace, all workloads running in the workspace need to be stopped to enable Private Link. Workloads also can't be started for another 20 minutes after making changes. From the [Databricks documentation](https://docs.databricks.com/en/security/network/classic/privatelink.html#step-3d-create-or-update-the-workspace-front-end-back-end-or-both):

   "After creating (or updating) a workspace, wait until it’s available for using or creating clusters. The workspace status stays at status RUNNING and the VPC change happens immediately. However, you cannot use or create clusters for another 20 minutes. If you create or use clusters before this time interval elapses, clusters do not launch successfully, fail, or could cause other unexpected behavior."

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once you've completed the setup in the Databricks environment, you can configure a private endpoint in dbt:

1. Navigate to **Settings** → **Create new project** → select **Databricks**.
2. You will see two radio buttons: **Public** and **Private**. Select **Private**.
3. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
4. Configure the remaining data platform details.
5. Test your connection and save it.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
