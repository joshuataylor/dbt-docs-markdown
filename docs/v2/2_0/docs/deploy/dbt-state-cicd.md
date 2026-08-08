# Setting up dbt State for non-interactive environments [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Login required | Usage-basedⓘ

In a non-interactive environment, dbt runs without a person available to complete authentication manually — for example, CI/CD pipelines (such as GitHub Actions, GitLab CI, and Jenkins) and production orchestration tools (such as Airflow and Prefect). Browser-based authentication isn't possible in these environments. Instead, dbt State authenticates using credentials provided through environment variables, allowing it to continue caching state and optimizing your builds.

dbt State automatically detects when it's running in a non-interactive environment. If valid credentials are not provided, dbt State disables itself and displays a warning, allowing your dbt commands to continue without caching.

There are two authentication methods depending on your setup:

* [**Service account token**](#service-token-dbt-platform) for dbt platform users
* [**OAuth client credentials**](#oauth-client-credentials) for standalone dbt State app users

## Service account token[​](#service-account-token "Direct link to Service account token")

For dbt platform users, you can authenticate dbt State with a [service token](../dbt-apis/service-tokens.md).

### Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before you begin, make sure you have:

* A dbt platform account.
* **Owner** or **Account Admin** permissions to create a service token.
* dbt State installed and configured. Refer to [Set up dbt State](./dbt-state-setup.md) for more information.

### Creating a service token[​](#creating-a-service-token "Direct link to Creating a service token")

To create a service account token in dbt platform, refer to [Generate service account tokens](../dbt-apis/service-tokens.md#generate-service-account-tokens). When adding permissions for the token, assign at least one of the following:

* **Owner**
* **Account Admin**
* **Job Admin**
* **Job Creator**
* **Job Runner** (recommended; provides the minimum access required for dbt State)
* **Developer**

### Configuring authentication[​](#configuring-authentication "Direct link to Configuring authentication")

Set the following environment variables in your orchestration environment:

```bash
DBT_CLOUD_TOKEN=YOUR_SERVICE_TOKEN
DBT_CLOUD_ACCOUNT_HOST=YOUR_ACCOUNT_HOST
DBT_CLOUD_ACCOUNT_ID=YOUR_ACCOUNT_ID
```

Replace `YOUR_SERVICE_TOKEN` with your service token, `YOUR_ACCOUNT_HOST` with your [account host](../platform/about-platform/access-regions-ip-addresses.md) (for example, `abc123.us1.dbt.com`), and `YOUR_ACCOUNT_ID` with your numeric account ID. Go to **Account settings** > **Account** to find your account ID and account host (the hostname from the **Access URL** field).

## OAuth client credentials[​](#oauth-client-credentials "Direct link to OAuth client credentials")

If you're using the standalone [dbt State web app](https://app.state.dbt.com/), authenticate with OAuth client credentials.

### Prerequisites[​](#prerequisites-1 "Direct link to Prerequisites")

* dbt State installed and configured. Refer to [Set up dbt State](./dbt-state-setup.md) for more information.
* A standalone dbt State account at [app.state.dbt.com](https://app.state.dbt.com/).
* An **Admin** or **Owner** role in your dbt State organization. Refer to [Roles and tab access](#roles-and-tab-access) for details.

## Roles and tab access[​](#roles-and-tab-access "Direct link to Roles and tab access")

The dbt State web app has four tabs under **Organization**:

| Tab         | Description                                                                       |
| ----------- | --------------------------------------------------------------------------------- |
| **Usage**   | View your project reuses and compute time saved once dbt State is enabled.        |
| **Users**   | Invite team members and grant or revoke access.                                   |
| **Billing** | View daily active target tables (DATTs) for the current billing period.           |
| **Clients** | Create and manage OAuth clients for CI/CD and other non-interactive environments. |

<br />

Your role determines which tabs you can access.

| Role          | Access                         | Notes                                                                                                                                                       |
| ------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Owner**     | Usage, Users, Billing, Clients | The user who created the organization is the Owner by default. An Owner can transfer their role to another user, which demotes the original Owner to Admin. |
| **Admin**     | Usage, Users, Billing, Clients | —                                                                                                                                                           |
| **Developer** | Usage                          | Default role when users are added.                                                                                                                          |

An existing **Owner** or **Admin** can grant or revoke admin access from the **Users** tab.

### Creating an OAuth client[​](#creating-an-oauth-client "Direct link to Creating an OAuth client")

1. In the [dbt State web app](https://app.state.dbt.com/), navigate to the **Clients** tab.
2. Click **Add OAuth Client**.
3. Enter a name and description for the new client and click **Create**.
4. Copy the client ID and secret to use in your environment configuration.

### Configuring OAuth authentication[​](#configuring-oauth-authentication "Direct link to Configuring OAuth authentication")

Once you have the client ID and secret, set the following environment variables in your environment. Using environment variables is the recommended approach as it keeps sensitive credentials out of your code repository.

```bash
DBT_ENGINE_STATE_OAUTH_CLIENT_ID=YOUR_CLIENT_ID
DBT_ENV_SECRET_STATE_OAUTH_CLIENT_SECRET=YOUR_CLIENT_SECRET
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with the values from your OAuth client.

## Verifying dbt State is active[​](#verifying-dbt-state-is-active "Direct link to Verifying dbt State is active")

1. Run any dbt transformation job in your orchestrated environment.

2. Check the log output. You should see a message like this, then the specific dbt State step status:

   ```text
   dbt State adapter: dbt-state v2.10.1 is enabled
   ```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](./dbt-state-about.md)
* [Set up dbt State](./dbt-state-setup.md)
* [dbt State configs](../../reference/resource-configs/dbt-state-configs.md)
* [Migrate from state-aware orchestration](./dbt-state-migration.md)
