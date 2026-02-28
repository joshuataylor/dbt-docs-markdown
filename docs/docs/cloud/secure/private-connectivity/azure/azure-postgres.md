# Configuring Private Link for Azure Database for Postgres Flexible Server

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

The following steps walk you through the setup of a Private Link endpoint for Azure Database for Postgres Flexible Server in a dbt multi-tenant environment.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->Azure Database<!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Configure Azure Private Link[​](#configure-azure-private-link "Direct link to Configure Azure Private Link")

From your Azure portal:

1. Navigate to your Azure Database for Postgres Flexible Server.

2. From the server overview, click **JSON view**.

3. Copy the value in the **Resource ID** field at the top of the pane.
   <br />
   <!-- -->
   The path format is: `/subscriptions/<subscription_uuid>/resourceGroups/<resource_group_name>/providers/Microsoft.DBforPostgreSQL/flexibleServers/<server_name>`.

4. Add the required information to the following template and submit your Azure Private Link request to [dbt Support](https://docs.getdbt.com/docs/dbt-support.md#dbt-cloud-support):

   <!-- -->

   ```text
     Subject: New Azure Multi-Tenant Private Link Request
   - Type: Azure Database for Postgres Flexible Server
   - Postgres Flexible Server name:
   - Azure Database for Postgres Flexible Server resource ID:
   - dbt Azure multi-tenant environment (EMEA):
   - Azure Postgres server region (for example, WestEurope, NorthEurope):
   ```

5. Once our Support team confirms the endpoint has been created, navigate to the Azure Database for Postgres Flexible Server in the Azure Portal and browse to **Settings** > **Networking**. In the **Private Endpoints** section, highlight the `dbt` named option and select **Approve**. Confirm with dbt Support that the connection has been approved so they can validate the connection and make it available for use in dbt.

## Create connection in dbt[​](#create-connection-in-dbt "Direct link to Create connection in dbt")

Once you've completed the setup in the Azure environment, you can configure a private endpoint in dbt:

1. Navigate to **Settings** → **Create new project** → select **Postgres**.
2. You will see two radio buttons: **Default Endpoint** and **PrivateLink Endpoint**. Select **PrivateLink Endpoint**.
3. Select the private endpoint from the dropdown (this will automatically populate the hostname/account field).
4. Configure the remaining data platform details.
5. Test your connection and save it.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
