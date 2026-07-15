# Configuring Databricks and Azure Private Link

Available to certain Enterprise tiers

This feature is available on the following dbt Enterprise tiers:

* Enterprise+
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a Databricks Azure Private Link endpoint in the dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Databricks<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

VNet injection required

Azure supports private endpoints for Databricks workspaces only when the workspace uses VNet injection (a customer-managed VNet). Workspaces on the default Azure-managed VNet reject private endpoint connections with a `NonVNetInjectedWorkspaceNotSupported` error.

Confirm your workspace uses VNet injection before you submit the request. If it doesn't, create a new workspace with VNet injection enabled first. For more information, refer to [Deploy Azure Databricks in your own virtual network](https://learn.microsoft.com/en-us/azure/databricks/security/network/classic/vnet-inject).

## Configure Azure Private Link[​](#configure-azure-private-link "Direct link to Configure Azure Private Link")

1. Navigate to your Azure Databricks workspace. The path format is: `/subscriptions/<subscription_uuid>/resourceGroups/<resource_group_name>/providers/Microsoft.Databricks/workspaces/<workspace_name>`.

2. From the workspace overview, click **JSON view**.

3. Copy the value in the `resource_id` field.

4. Add the required information to the following template and submit your Azure Private Link request to [dbt Support](mailto:support@getdbt.com):

    Support request email template

   ```text
   Subject: New Azure Multi-Tenant Private Link Request

   - Type: Databricks
   - dbt platform account URL:
   - Databricks instance name:
   - Azure Databricks Workspace URL (for example, adb-################.##.azuredatabricks.net)
   - Databricks Azure resource ID: /subscriptions/SUB_ID/resourceGroups/RG/providers/Microsoft.Databricks/workspaces/WORKSPACE_NAME
     - Use the full ARM resource ID, replacing SUB_ID, RG, and WORKSPACE_NAME with your values
   - dbt Azure multi-tenant environment (US or EMEA):
   - Azure Databricks workspace region (for example, WestEurope, NorthEurope)
   ```

   dbt Labs will work on your behalf to complete the private connection setup. Please allow 3-5 business days for this process to complete. Support will contact you when the endpoint is available.

5. Once our Support team confirms the resources are available in the Azure portal, navigate to the Azure Databricks Workspace and browse to **Networking** > **Private Endpoint Connections**. Then, highlight the `dbt` named option and select **Approve**.

   DNS impact when approving the first private endpoint

   Approving the first private endpoint connection for an Azure Databricks workspace modifies the workspace's DNS resolution. Azure inserts a `privatelink.azuredatabricks.net` DNS `A` record that redirects the workspace URL to the private IP address instead of the public endpoint.

   Any clients — including other dbt platform projects, tools, or users — that connect to this workspace over the public internet **may lose connectivity** after this change. Coordinate this change with all teams that use the workspace before approving. For more information, see [Azure services DNS zone configuration](https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-dns#azure-services-dns-zone-configuration).

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once you've completed the setup in the Databricks environment, you can configure a private endpoint in dbt:

1. Navigate to **Account Settings** → **Projects** → **Create new project**.
2. Under the **Configure your development environment step** section, select **Add new connection**.
3. You'll be directed to the **Add new connection** page, select **Databricks**.
4. Under the **Settings** section, select **PrivateLink Endpoint**.
5. Select the private endpoint from the dropdown (this automatically populates the hostname/account field).
6. Configure the remaining data platform details.
7. Test your connection and save it.
