# Setting up authentication for non-interactive environments

Login required | Usage-based

In a non-interactive environment, dbt runs without a person available to complete authentication manually — for example, CI/CD pipelines (such as GitHub Actions, GitLab CI, and Jenkins) and production orchestration tools (such as Airflow and Prefect). Browser-based authentication isn't possible in these environments. Instead, dbt State authenticates using credentials provided through environment variables, allowing it to continue caching state and optimizing your builds.

dbt State automatically detects when it's running in a non-interactive environment. If valid credentials are not provided, dbt State disables itself and displays a warning, allowing your dbt commands to continue without caching.

Running dbt in a non-interactive environment requires a service account token for authentication. A dbt platform account is required to generate a service token; if you don't have one yet, refer to [Set up dbt State](./dbt-state-setup.md) to create a free account before proceeding.

If you previously signed up for dbt State through the standalone app (`app.state.dbt.com`) and haven't migrated to dbt platform yet, refer to [OAuth client credentials](./dbt-state-oauth.md).

## Prerequisites

Before you begin, make sure you have:

* A dbt platform account.
* **Owner** or **Account Admin** permissions to create a service token.
* dbt State installed and configured. Refer to [Set up dbt State](./dbt-state-setup.md) for more information.

## Creating a service token

To create a service account token in dbt platform, refer to [Generate service account tokens](../dbt-apis/service-tokens.md#generate-service-account-tokens). When adding permissions for the token, assign at least one of the following:

* **Account Admin**

The following are available on Enterprise plans only:

* **Owner**
* **Job Admin**
* **Job Creator**
* **Job Runner** (Recommended; provides the minimum access required for dbt State)
* **Developer**

## Configuring authentication

Set the following environment variables in your orchestration environment:

```bash
DBT_CLOUD_TOKEN=YOUR_SERVICE_TOKEN
DBT_CLOUD_ACCOUNT_HOST=YOUR_ACCOUNT_HOST
DBT_CLOUD_ACCOUNT_ID=YOUR_ACCOUNT_ID
```

Replace `YOUR_SERVICE_TOKEN` with your service token, `YOUR_ACCOUNT_HOST` with your [account host](../platform/about-platform/access-regions-ip-addresses.md) (for example, `abc123.us1.dbt.com`), and `YOUR_ACCOUNT_ID` with your numeric account ID. Go to **Account settings** > **Account** to find your account ID and account host (the hostname from the **Access URL** field).

### Network requirements

If your network or environment restricts outbound traffic, allow the following URLs so dbt State can authenticate and communicate with the dbt platform:

| URL                  | Description                                                                                                                                                  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `auth.state.dbt.com` | dbt State authentication                                                                                                                                     |
| `api.state.dbt.com`  | dbt State API                                                                                                                                                |
| `abc123.us1.dbt.com` | Your dbt platform [access URL](../platform/about-platform/access-regions-ip-addresses.md) (Replace with your account's access URL) |

Once authentication is configured, [verify dbt State is active](#verifying-dbt-state-is-active).

## Verifying dbt State is active

1. Run any dbt transformation job in your orchestrated environment.

2. Check the log output. You should see a message like this, then the specific dbt State step status:

   ```text
   dbt State adapter: dbt-state v2.10.1 is enabled
   ```

## Related docs

* [About dbt State](./dbt-state-about.md)
* [Set up dbt State](./dbt-state-setup.md)
* [dbt State configs](../../reference/resource-configs/dbt-state-configs.md)
* [Migrate from state-aware orchestration](./dbt-state-migration.md)
