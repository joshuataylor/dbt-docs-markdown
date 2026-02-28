# Configuring Private Link for Azure Synapse

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a Private Link endpoint for Azure Synapse in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Azure Synapse<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Configure Azure Private Link[​](#configure-azure-private-link "Direct link to Configure Azure Private Link")

From your Azure portal:

1. Navigate to your Azure Synapse workspace.

2. From the workspace overview, click **JSON view**.

3. Copy the value in the **Resource ID** field at the top of the pane.
   <br />
   <!-- -->
   The path format is: `/subscriptions/<subscription_uuid>/resourceGroups/<resource_group_name>/providers/Microsoft.Synapse/workspaces/<workspace_name>`.

4. Add the required information to the following template and submit your Azure Private Link request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

   <!-- -->

   ```text
     Subject: New Azure Multi-Tenant Private Link Request
   - Type: Azure Synapse
   - Server name:
   - Azure Synapse workspace resource ID:
   - dbt Azure multi-tenant environment (EMEA):
   - Azure Synapse workspace region (for example, WestEurope, NorthEurope):
   ```

5. Once our Support team confirms the endpoint has been created, navigate to the Azure Synapse workspace in the Azure Portal and browse to **Security** > **Private endpoint connections**. In the **Private endpoint connections** table, highlight the `dbt` named option and select **Approve**. Confirm with dbt Support that the connection has been approved so they can validate the connection and make it available for use in dbt.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once you've completed the step above, you can configure a private endpoint in dbt:

1. Navigate to **Settings** → **Create new project** → select **Synapse**.
2. You will see two radio buttons: **Default Endpoint** and **PrivateLink Endpoint**. Select **PrivateLink Endpoint**.
3. Select the private endpoint from the dropdown (this will automatically populate the hostname/account field).
4. Configure the remaining data platform details.
5. Test your connection and save it.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
