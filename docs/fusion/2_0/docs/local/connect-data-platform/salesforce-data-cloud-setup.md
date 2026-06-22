# Salesforce Data 360 setup [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

This `dbt-salesforce` adapter is available via the dbt Fusion engine CLI. To access the adapter, [install Fusion](https://docs.getdbt.com/docs/fusion/about-fusion-install.md). We recommend using the [VS Code Extension](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) as the development interface. dbt platform support coming soon.

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

## Configure Fusion[​](#configure-fusion "Direct link to Configure Fusion")

To connect dbt to Salesforce Data 360, set up your `profiles.yml`. Refer to the following configuration:

\~/.dbt/profiles.yml

```yaml
company-name:
  target: dev
  outputs:
    dev:
      type: salesforce
      method: jwt_bearer
      client_id: [Consumer Key of your Data 360 app]
      private_key_path: [local file path of your server key]
      login_url: "https://login.salesforce.com"
      username: [username on the Data 360 Instance]
```

| Profile field      | Required | Description                                                    | Example                                                       |
| ------------------ | -------- | -------------------------------------------------------------- | ------------------------------------------------------------- |
| `method`           | Yes      | Authentication Method. Currently, only `jwt_bearer` supported. | `jwt_bearer`                                                  |
| `client_id`        | Yes      | This is the `Consumer Key` from your connected app secrets.    |                                                               |
| `private_key_path` | Yes      | File path of the `server.key` file in your computer.           | `/Users/dbt_user/Documents/server.key`                        |
| `login_url`        | Yes      | Login URL of the Salesforce instance.                          | [https://login.salesforce.com](https://login.salesforce.com/) |
| `username`         | Yes      | Username on the Data 360 Instance.                             | <dbt_user@dbtlabs.com>                                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## More information[​](#more-information "Direct link to More information")

Find Salesforce-specific configuration information in the [Salesforce adapter reference guide](https://docs.getdbt.com/reference/resource-configs/data-cloud-configs.md).
