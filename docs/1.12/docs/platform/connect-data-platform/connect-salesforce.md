# Connect Salesforce Data 360 [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") Fusion compatible

The dbt Fusion engine in dbt platform supports connecting to Salesforce Data 360.

<!-- -->

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before you can connect dbt to Salesforce Data 360, you need the following:

* A Data 360 instance.
* The `server.key` private key file. For more information, refer to [Generating a private key and certificate](#generating-a-private-key-and-certificate).
* An external client app configured for JSON Web Token (JWT) Bearer token flow. For more information, refer to [Setting up the external client app](#setting-up-the-external-client-app).
* `Data Cloud Architect` and `Data Cloud User` permissions.

### Generating a private key and certificate[​](#generating-a-private-key-and-certificate "Direct link to Generating a private key and certificate")

Before creating the external client app, generate a private key (`server.key`) and a self-signed certificate (`server.crt`) in Salesforce. Salesforce uses the certificate to verify the JWT Bearer token that dbt sends. Refer to [Create a Private Key and Self-Signed Digital Certificate](https://developer.salesforce.com/docs/atlas.en-us.252.0.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_key_and_cert.htm) for instructions.

note

It is recommended to generate these credentials under a **service user account** rather than an individual user account to simplify credential management.

### Creating the external client app[​](#creating-the-external-client-app "Direct link to Creating the external client app")

To authenticate dbt using JWT Bearer token flow, you must create and configure an external client app in Salesforce Data 360.

Follow the steps in [Create a Local External Client App](https://help.salesforce.com/s/articleView?id=xcloud.create_a_local_external_client_app.htm\&type=5) to create an external client app. For Step 6, make sure to select **Packaged**.

Then, configure the OAuth settings for the app:

1. Under **API (Enable OAuth Settings)**, select **Enable OAuth**.

2. Set the **Callback URL** to `https://login.salesforce.com/services/oauth2/callback`.

3. Under **OAuth Scopes**, add the following to the **Selected OAuth Scopes** list:

   <!-- -->

   * `api` - To manage user data via APIs
   * `refresh_token`, `offline_access` - To perform requests at any time, even when the user is offline or tokens have expired
   * `cdp_query_api` - To perform SQL queries on Data 360 data

4. Under **Flow Enablement**, select **Enable JWT Bearer Flow**.

5. Click **Upload Files** and upload the `server.crt` file created in [Generating a private key and certificate](#generating-a-private-key-and-certificate).

6. Click **Create**.

7. On the **Policies** tab, select **Edit**.

8. Under **OAuth Policies** > **Plugin Policies** > **Permitted Users**, select **Admin approved users are pre-authorized**. A list of **Select Profiles and Select Permission Sets** appears.

9. Selct the profiles and permission sets that should have access to the external client app.

10. Under **App Authorization**, set **IP Relaxation** to **Relax IP Restrictions**.

11. Click **Save**.

## Connection fields[​](#connection-fields "Direct link to Connection fields")

To connect to Salesforce Data 360, configure the connection settings and provide your credentials.

### Connection[​](#connection "Direct link to Connection")

Configure the following fields to set up your Salesforce Data 360 connection:

| Field                      | Description                                                                                         | Example                                  |
| -------------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| Connection name            | A name for your Salesforce Data 360 connection.                                                     |                                          |
| Auth method                | The authentication method used to connect to Salesforce Data 360. JWT is the only supported method. | JWT Bearer Flow (default)                |
| Login URL                  | The Salesforce instance URL.                                                                        | `https://login.salesforce.com` (default) |
| Database                   | (Optional) The Salesforce Data 360 database to connect to.                                          |                                          |
| Data Transform Run Timeout | (Optional) The timeout duration (in milliseconds) for data transformation runs.                     |                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Credentials[​](#credentials "Direct link to Credentials")

Enter the following credentials to authenticate with Salesforce Data 360:

| Field       | Description                                         | Example                 |
| ----------- | --------------------------------------------------- | ----------------------- |
| Username    | Your Salesforce Data 360 username.                  | <user.name@example.com> |
| Client ID   | The `Consumer Key` of the Salesforce Data 360 app.  |                         |
| Private Key | The private key for JWT bearer flow authentication. |                         |

* For **Username**, use your Salesforce account username. Authentication will only succeed if that username has one of the profiles or permission sets added to the external client app in [step 9](#creating-the-external-client-app).
* For **Client ID**, get the Consumer Key from your external client app in Salesforce. Go to **Settings** > **OAuth Settings** > **Consumer Key and Secret**. Copy the **Consumer Key** and paste to the **Client ID** field.
* The **Private Key** is the `server.key` file created in [Generating a private key and certificate](#generating-a-private-key-and-certificate).

## Authentication method[​](#authentication-method "Direct link to Authentication method")

Salesforce Data 360 supports JWT bearer authentication only. JWT bearer flow is a machine-to-machine authentication method that uses a private key file and Consumer Key (Client ID) to authenticate without requiring user interaction.

## Configuration[​](#configuration "Direct link to Configuration")

To learn how to optimize performance with data platform-specific configurations in dbt platform, refer to [Salesforce Data 360 configurations](https://docs.getdbt.com/reference/resource-configs/data-cloud-configs.md).

## Limitations[​](#limitations "Direct link to Limitations")

The following dbt features are not yet supported for Salesforce Data 360 connections in dbt platform:

* [Seeds](https://docs.getdbt.com/docs/build/seeds.md)
  * As a workaround, log in to Salesforce and upload the CSV file manually.
* [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md)
* [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
