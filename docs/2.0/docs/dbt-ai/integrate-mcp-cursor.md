# Integrate Cursor with dbt MCP

[Cursor](https://docs.cursor.com/context/model-context-protocol) is an AI-powered code editor, powered by Microsoft Visual Studio Code (VS Code).

After setting up your MCP server, you connect it to Cursor. Log in to Cursor and follow the steps that align with your use case.

## Set up with local dbt MCP server[​](#set-up-with-local-dbt-mcp-server "Direct link to Set up with local dbt MCP server")

Choose your setup based on your workflow:

* OAuth for dbt platform connections
* CLI only if using dbt Core or the dbt Fusion engine locally.
* Configure environment variables if you're using them in your dbt platform account.

### OAuth or CLI[​](#oauth-or-cli "Direct link to OAuth or CLI")

Click one of the following application links with Cursor open to automatically configure your MCP server:

* CLI only (dbt Core and Fusion)
* OAuth with dbt platform

Local configuration for users who only want to use dbt commands with dbt Core or dbt Fusion engine (no dbt platform features).

[Add dbt Core or Fusion to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJlbnYiOnsiREJUX1BST0pFQ1RfRElSIjoiL3BhdGgvdG8veW91ci9kYnQvcHJvamVjdCIsIkRCVF9QQVRIIjoiL3BhdGgvdG8veW91ci9kYnQvZXhlY3V0YWJsZSJ9LCJjb21tYW5kIjoidXZ4IiwiYXJncyI6WyJkYnQtbWNwIl19)

After clicking:

1. Update `DBT_ENGINE_PROJECT_DIR` with the full path to your dbt project (the folder containing `dbt_project.yml`).

2. Update `DBT_PATH` with the full path to your dbt executable:

   <!-- -->

   * macOS/Linux: Run `which dbt` in Terminal.
   * Windows: Run `where dbt` in Command Prompt or PowerShell.

3. Save the configuration.

Configuration settings for users who want OAuth authentication with the dbt platform [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing").

Before you begin, make sure your account admin has enabled AI features on your dbt platform account. Refer to [Enable dbt AI](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for more info.

* [dbt platform only](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJlbnYiOnsiREJUX0hPU1QiOiJodHRwczovLzx5b3VyLWRidC1ob3N0LXdpdGgtY3VzdG9tLXN1YmRvbWFpbj4iLCJESVNBQkxFX0RCVF9DTEkiOiJ0cnVlIn0sImNvbW1hbmQiOiJ1dngiLCJhcmdzIjpbImRidC1tY3AiXX0%3D)
* [dbt platform + CLI](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJlbnYiOnsiREJUX0hPU1QiOiJodHRwczovLzx5b3VyLWRidC1ob3N0LXdpdGgtY3VzdG9tLXN1YmRvbWFpbj4iLCJEQlRfUFJPSkVDVF9ESVIiOiIvcGF0aC90by9wcm9qZWN0IiwiREJUX1BBVEgiOiJwYXRoL3RvL2RidC9leGVjdXRhYmxlIn0sImNvbW1hbmQiOiJ1dngiLCJhcmdzIjpbImRidC1tY3AiXX0%3D)

After clicking:

1. Replace `<your-dbt-host-with-custom-subdomain>` with your actual host (for example, `abc123.us1.dbt.com`).
2. (For dbt platform + CLI) Update `DBT_ENGINE_PROJECT_DIR` and `DBT_PATH` as described above.
3. Save the configuration.

### Custom environment variables[​](#custom-environment-variables "Direct link to Custom environment variables")

Use this method if you need custom environment variables or prefer to use service tokens. Refer to the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md) for the complete list of available environment variables for the local MCP server.

1. Click the following link with Cursor open:

   [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJjb21tYW5kIjoidXZ4IiwiYXJncyI6WyJkYnQtbWNwIl0sImVudiI6e319)

2. In the template, add your environment variables to the `env` section based on your needs.

3. Save the configuration.

#### Using an `.env` file[​](#using-an-env-file "Direct link to using-an-env-file")

If you prefer to manage environment variables in a separate file, put the `.env` file in your *dbt project root* (same folder as `dbt_project.yml`). Click this link:

[Add to Cursor (with .env file)](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt-mcp\&config=eyJjb21tYW5kIjoidXZ4IC0tZW52LWZpbGUgPGVudi1maWxlLXBhdGg%252BIGRidC1tY3AifQ%3D%3D)

Then update `env-file-path` with the absolute path to your `.env` file (for example, `/absolute/path/to/your-dbt-project/.env`).

## Set up with remote dbt MCP server[​](#set-up-with-remote-dbt-mcp-server "Direct link to Set up with remote dbt MCP server")

Remote MCP supports **OAuth** or **token-based** headers.

* *OAuth is in private beta for Enterprise and Enterprise+ accounts.*
* For either method, the MCP URL is `https://<Access URL>/api/ai/v1/mcp`. You can find the URL in dbt platform under **Account settings** → **Access URLs** → **MCP Endpoint URL**.

**Before you connect**

* Your MCP client must support OAuth for HTTP-based MCP servers. If it doesn't, use [token-based authentication](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication) instead.
* On first connect, your client opens a browser for sign-in. dbt then shows a consent screen with the scopes (the specific permissions the client is allowed to use) it's requesting — see [Scopes and consent](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent) for what each scope means.
* Most modern MCP clients self-register on first connect via [dynamic registration (RFC 7591)](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#dynamic-registration). Clients that don't support it need an admin to register them in **Account settings → Integrations → App integrations**. See [Manual registration](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#manual-registration).

For the full flow, sessions, and limitations, refer to [OAuth (remote MCP)](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#oauth-remote-mcp).

The deeplink below configures **token-based** authentication (URL and headers). For OAuth setup, follow the [remote MCP quickstart](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-remote.md#5-configure-your-mcp-client).

1. Click the following application link with Cursor open:

   [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJ1cmwiOiJodHRwczovLzxob3N0Pi9hcGkvYWkvdjEvbWNwLyIsImhlYWRlcnMiOnsiQXV0aG9yaXphdGlvbiI6InRva2VuIDx0b2tlbj4iLCJ4LWRidC1wcm9kLWVudmlyb25tZW50LWlkIjoiPHByb2QtaWQ%252BIn19)

2. Provide your URL/headers by updating the **host**, **production environment ID**, and **service token** in the template. Use `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp` as the server URL (no trailing slash).

   IDs are integers, not URLs

   `DBT_PROD_ENV_ID`, `DBT_USER_ID`, and `DBT_DEV_ENV_ID` must be numeric IDs (for example, `54321`), not full URLs.

   `DBT_HOST` field accepts the `https://` prefix and without the `https://` prefix. The following are valid examples:

   ```bash
   DBT_HOST=https://ab123.us1.dbt.com
   DBT_HOST=ab123.us1.dbt.com
   ```

3. Save, and now you have access to the dbt MCP server!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
