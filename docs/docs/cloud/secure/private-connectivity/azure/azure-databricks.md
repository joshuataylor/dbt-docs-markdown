# Configuring Databricks and Azure Private Link

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a Databricks Azure Private Link endpoint in the dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Databricks<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Configure Azure Private Link[​](#configure-azure-private-link "Direct link to Configure Azure Private Link")

1. Navigate to your Azure Databricks workspace. The path format is: `/subscriptions/<subscription_uuid>/resourceGroups/<resource_group_name>/providers/Microsoft.Databricks/workspaces/<workspace_name>`.

2. From the workspace overview, click **JSON view**.

3. Copy the value in the `resource_id` field.

4. Add the required information to the following template and submit your Azure Private Link request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

   <!-- -->

   ```text
     Subject: New Azure Multi-Tenant Private Link Request
   - Type: Databricks
   - Databricks instance name:
   - Azure Databricks Workspace URL (for example, adb-################.##.azuredatabricks.net)
   - Databricks Azure resource ID:
   - dbt Azure multi-tenant environment (EMEA):
   - Azure Databricks workspace region (like WestEurope, NorthEurope):
   ```

5. Once our Support team confirms the resources are available in the Azure portal, navigate to the Azure Databricks Workspace and browse to **Networking** > **Private Endpoint Connections**. Then, highlight the `dbt` named option and select **Approve**.

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
