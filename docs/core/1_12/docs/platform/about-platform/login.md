# Log in to dbt platform

<https://login.dbt.com> is the universal entry point for dbt platform accounts with a static access URL (for example, `abc123.us1.dbt.com`). It sits outside any specific account and maps your email to the accounts you can access across instances and regions.

You can also sign in directly with your account **Access URL** if you already know it, for example, `abc123.us1.dbt.com`. Refer to [Access, regions, and IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) for the full list of access URLs by region.

info

`login.dbt.com` is currently available for multi-tenant accounts with an account-specific domain (for example, `abc123.us1.dbt.com`). Support for single-tenant accounts is coming soon. In the meantime, single-tenant users can sign in directly using their account **Access URL** (like `MY_COMPANY.us1.dbt.com`).

OAuth clients such as [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md), the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md?version=2.0), and [dbt MCP](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md) have not yet been updated to use `login.dbt.com` and continue to authenticate through their account [**Access URL**](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

## How login works[​](#how-login-works "Direct link to How login works")

1. Go to <https://login.dbt.com>.

2. Enter your email and you will receive a verification code to log in if you have access to a dbt account. If you don't have a dbt account, you can create one on the same login page.

3. Check your email for the verification code, then enter it on `login.dbt.com` to confirm your identity. The code expires after 5 minutes or 3 attempts.
   <!-- -->
   * If you didn't receive a code, check your spam folder or request a new one. You may not receive a verification code if your email is not associated with a dbt account.

4. Once you verify, dbt platform looks up which accounts are associated with your email address:

   <!-- -->

   * If you have access to multiple accounts, you see a list of accounts including their region and tenancy. Select the one you want to open.
   * If you have access to only one account, you're redirected to that account directly.

5. Authenticate using the method configured for that account (password, SSO, or MFA).

note

If no accounts are found for your email, contact [dbt Support](mailto:support@getdbt.com) or your account admin. You can also sign in directly at your account [**Access URL**](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

[![Login to dbt platform using the login.dbt.com page.](/img/docs/dbt-platform/login-email-page.png?v=2 "Login to dbt platform using the login.dbt.com page.")](#)Login to dbt platform using the login.dbt.com page.

[![Verify code to login to dbt platform](/img/docs/dbt-platform/login-email-verify-code.png?v=2 "Verify code to login to dbt platform")](#)Verify code to login to dbt platform

[![Select account to login to dbt platform](/img/docs/dbt-platform/login-account-list.png?v=2 "Select account to login to dbt platform")](#)Select account to login to dbt platform

## SSO behavior[​](#sso-behavior "Direct link to SSO behavior")

After you select an account, dbt platform applies that account's authentication requirements. If SSO is enforced for the account, you'll be redirected to your identity provider (IdP) to complete authentication. Refer to [SSO overview](https://docs.getdbt.com/docs/platform/manage-access/sso-overview.md) for more information about SSO configuration.

## Admin settings[​](#admin-settings "Direct link to Admin settings")

Account administrators can control whether users can discover an account through `login.dbt.com`.

1. In dbt platform, account admins can go to [**Account settings**](https://docs.getdbt.com/docs/platform/account-settings.md#enable-global-account-discovery) and scroll to **Settings**.
2. Navigate to the bottom and check the **Enable global account discovery** option to allow users will be able to find the account by entering their email at `login.dbt.com`.

* When an admin turns off the option, users must know their account **Access URL** to sign in and won't see the account in the discovery list.

To change this setting, go to **Account settings** and select or clear **Enable global account discovery**. For details, refer to [Account settings](https://docs.getdbt.com/docs/platform/account-settings.md#enable-global-account-discovery).
