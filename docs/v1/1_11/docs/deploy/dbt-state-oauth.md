# OAuth client credentials for dbt State Deprecated

Login required | Usage-based

The dbt State app is deprecated

The standalone dbt State app (`app.state.dbt.com`) is no longer accepting new users. If you have an existing dbt State app account, [create a free dbt platform account](http://us1.dbt.com/register?_dbtsrc=dbt-state) to continue using dbt State — your free trial will be extended upon account creation.

If you're using the standalone [dbt State web app](https://app.state.dbt.com/), authenticate dbt State with OAuth client credentials.

## Prerequisites

* dbt State installed and configured. Refer to [Set up dbt State](./dbt-state-setup.md) for more information.
* A standalone dbt State account at [app.state.dbt.com](https://app.state.dbt.com/).
* An **Admin** or **Owner** role in your dbt State organization. Refer to [Roles and tab access](#roles-and-tab-access) for details.

## Roles and tab access

The dbt State web app has four tabs under **Organization**:

| Tab         | Description                                                                       |
| ----------- | --------------------------------------------------------------------------------- |
| **Usage**   | View your project reuses and compute time saved once dbt State is enabled.        |
| **Users**   | Invite team members and grant or revoke access.                                   |
| **Billing** | View daily active target tables (DATTs) for the current billing period.           |
| **Clients** | Create and manage OAuth clients for CI/CD and other non-interactive environments. |

Your role determines which tabs you can access.

| Role          | Access                         | Notes                                                                                                                                                       |
| ------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Owner**     | Usage, Users, Billing, Clients | The user who created the organization is the Owner by default. An Owner can transfer their role to another user, which demotes the original Owner to Admin. |
| **Admin**     | Usage, Users, Billing, Clients | —                                                                                                                                                           |
| **Developer** | Usage                          | Default role when users are added.                                                                                                                          |

An existing **Owner** or **Admin** can grant or revoke admin access from the **Users** tab.

## Creating an OAuth client

1. In the [dbt State web app](https://app.state.dbt.com/), navigate to the **Clients** tab.
2. Click **Add OAuth Client**.
3. Enter a name and description for the new client and click **Create**.
4. Copy the client ID and secret to use in your environment configuration.

## Configuring OAuth authentication

Once you have the client ID and secret, set the following environment variables in your environment. Using environment variables is the recommended approach as it keeps sensitive credentials out of your code repository.

```bash
DBT_ENGINE_STATE_OAUTH_CLIENT_ID=YOUR_CLIENT_ID
DBT_ENV_SECRET_STATE_OAUTH_CLIENT_SECRET=YOUR_CLIENT_SECRET
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with the values from your OAuth client.

Once configured, [verify dbt State is active](./dbt-state-cicd.md#verifying-dbt-state-is-active).

## Related docs

* [About dbt State](./dbt-state-about.md)
* [Set up dbt State](./dbt-state-setup.md)
* [Setting up authentication for non-interactive environments](./dbt-state-cicd.md)
