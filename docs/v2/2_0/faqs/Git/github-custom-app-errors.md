# Why does dbt reject my custom GitHub application details?

dbt validates your GitHub application against GitHub before saving it, so an invalid configuration is rejected instead of quietly breaking your team's connection. If you see an error when you save, check these in order:

* **App ID**: Use the **App ID** from your GitHub app's settings page, not the client ID or the installation ID.
* **Private key**: Paste the entire contents of the `.pem` file, including the `-----BEGIN RSA PRIVATE KEY-----` and `-----END RSA PRIVATE KEY-----` lines. If you lost the file, generate a new private key in GitHub.
* **Client ID**: Paste the **Client ID** from your GitHub app.
* **GitHub base URL**: Use the hostname of your GitHub instance with no trailing slash and no `/api/v3` path, for example `https://github.yourgreatcompany.com`.
* **Client secret**: GitHub only shows this once. If you didn't copy it, generate a new one.
* **Network access**: Your GitHub instance must be reachable from dbt. If you use [IP restrictions](../../docs/platform/secure/ip-restrictions.md), confirm the relevant CIDRs are allowed.

For setup steps, refer to [Custom GitHub application](../../docs/platform/git/connect-github.md#custom-github-application), available in private beta.
