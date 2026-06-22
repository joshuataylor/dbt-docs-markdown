# About dbt login

Use dbt login to authenticate once and unlock gated features across dbt tools.

info

Available in dbt v1.12 and v2.0 and later.

Run `dbt login` from the command line to unlock advanced dbt features. It'll open browser-based authentication where you can sign in to your existing dbt platform account or create a free one — no credit card required!

Run [`dbt login status`](#dbt-login-status) to view your current authentication status.

## When to use dbt login[​](#when-to-use-dbt-login "Direct link to When to use dbt login")

`dbt login` is intended for [dbt State](#dbt-login-with-dbt-state) or local development ([interactive](#interactive-authentication)) on macOS, Linux, and Windows, where you can complete authentication in a browser.

It does not support non-interactive authentication. For [non-interactive](#non-interactive-environments) environments, such as CI/CD jobs, scheduled jobs, or external orchestrators, use a service token instead to access advanced features.

### What dbt login unlocks[​](#what-dbt-login-unlocks "Direct link to What dbt login unlocks")

Use `dbt login` to unlock advanced features including:

* advanced features in the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [dbt State](#dbt-login-with-dbt-state)
* advanced features in v2.0 CLI

Refer to [VS Code extension features](https://docs.getdbt.com/docs/fusion/fusion-availability.md?version=1.13#dbt-vs-code-extension-features) for the full list of features and their availability.

## Before you log in[​](#before-you-log-in "Direct link to Before you log in")

Downloading the dbt VS Code extension gives you 14 days to try [advanced features](https://docs.getdbt.com/docs/fusion/fusion-availability.md#dbt-vs-code-extension-features) — no account needed. After the trial ends, sign in to or create a free dbt platform account to keep using them. The vast majority of features keep working either way.

This 14-day trial applies to the dbt VS Code extension only. It's separate from the [dbt platform trial](https://www.getdbt.com/pricing) and doesn't require a credit card or a paid plan.

A free dbt platform account keeps advanced features working after your trial ends and carries your access across all your dbt tools — the CLI, the VS Code extension, and dbt State — so you only log in once. No credit card required.

Run `dbt login` to create a free account, or log in to an existing one. Logging in is simply how dbt confirms your access to advanced features in local development.

Note that this is separate from [dbt platform user license types](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md?version=2.0\&name=Fusion) (such as Developer or Analyst), which controls what you can do *inside* dbt platform.

Refer to [VS Code extension features](https://docs.getdbt.com/docs/fusion/fusion-availability.md#dbt-vs-code-extension-features) for the full list of features and their availability.

<!-- -->

## Authentication methods[​](#authentication-methods "Direct link to Authentication methods")

`dbt login` is interactive only. It opens a browser-based sign-in flow and stores your session locally.

You can log into dbt either using [interactive](#interactive-authentication) or [non-interactive](#non-interactive-environments) methods.

### Interactive authentication[​](#interactive-authentication "Direct link to Interactive authentication")

Use `dbt login` when you're developing locally in a terminal or IDE and can complete sign-in in a browser.

* When you run `dbt login`, dbt opens a browser-based authentication flow. After you complete authentication, dbt stores your login state locally and confirms that you're signed in.
* After you sign in, dbt can use your login state to unlock advanced features across the dbt CLI, VS Code extension, and dbt State.
* Your session stays active across terminal sessions, and dbt refreshes it automatically while you're active. As long as you use dbt at least once every 7 days, you stay signed in. If you're inactive for longer, dbt prompts you to run `dbt login` again. Refer to [Staying signed in](#staying-signed-in) for details.

Run `dbt login` from your **local terminal** (not in the dbt platform UI).

**Example (interactive):**

```shell
dbt login
dbt login status
```

Complete the browser prompts to sign in or register for a dbt platform account.

### Non-interactive environments[​](#non-interactive-environments "Direct link to Non-interactive environments")

Non-interactive means dbt runs without a person at a keyboard to open a browser — for example, a GitHub Actions workflow, a cron job, or a scheduled dbt platform job. There is no separate "non-interactive login" command. You do not run `dbt login` in these environments.

If you are new to dbt, start with [interactive authentication](#interactive-authentication). You only need the guidance in this section when you automate dbt runs.

#### Authenticate with a service token[​](#authenticate-with-a-service-token "Direct link to Authenticate with a service token")

In non-interactive environments (such as CI/CD jobs, scheduled jobs, or external orchestrators like Airflow), use a [service token](https://docs.getdbt.com/docs/dbt-apis/service-tokens.md) instead of `dbt login`. The service token does two jobs:

* authenticates the run
* unlocks access to advanced features, so they work in your pipeline the same way they do locally.

Unlike interactive `dbt login`, service tokens don't expire, so there's nothing to refresh. Rotate them yourself in dbt platform when you need to.

Set the following [environment variables](https://docs.getdbt.com/docs/build/environment-variables.md#special-environment-variables) so dbt can authenticate and unlock that access:

| Environment variable     | Description                                                                                                                                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `DBT_CLOUD_ACCOUNT_HOST` | Your dbt platform [access URL](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) (for example, `abc123.us1.dbt.com`).                                         |
| `DBT_CLOUD_ACCOUNT_ID`   | Your dbt platform account ID.                                                                                                                                                                    |
| `DBT_CLOUD_TOKEN`        | The service token value. Create a service token with a permission set that includes feature licensing access, such as `Job Runner`, `Job Creator`, `Job Admin`, `Developer`, or `Account Admin`. |
| `DBT_CLOUD_PROJECT_ID`   | Your dbt platform project ID.                                                                                                                                                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

```shell
export DBT_CLOUD_ACCOUNT_HOST=abc123.us1.dbt.com
export DBT_CLOUD_ACCOUNT_ID=12345
export DBT_CLOUD_TOKEN=dbtc_xxxxxxxx
export DBT_CLOUD_PROJECT_ID=67890
```

<!-- -->

## How shared login works across dbt tools[​](#how-shared-login-works-across-dbt-tools "Direct link to How shared login works across dbt tools")

* Use `dbt login` to authenticate with [dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md). You can start the sign-in flow from the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md):
* If you run `dbt login` from the CLI, the dbt VS Code extension uses that login in your next extension session.
* If you sign in from the dbt VS Code extension, you can use that login the next time you run a login-gated command.

When you run a command or use a feature that requires authentication, dbt checks your current login state. If you're signed in, the feature runs. If you're not signed in, dbt tells you which feature requires authentication and prompts you to run `dbt login`.

For the VS Code extension registration flow, refer to [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md).

## Staying signed in[​](#staying-signed-in "Direct link to Staying signed in")

Once you log in, dbt keeps you signed in automatically — you usually won't notice it at all.

You stay signed in as long as you use dbt at least once every 7 days. If you're inactive for longer than that, dbt might ask you to log in again in your next session to ensure security.

If your access expires, run `dbt login` to sign back in.

* On the 14-day trial without a dbt account? Create a free account with `dbt login` — it's the best way to use the [full set of features](https://docs.getdbt.com/docs/fusion/fusion-availability.md#dbt-vs-code-extension-features) and get the most out of the extension.
* Not ready to run `dbt login`? No worries — continue using the vast majority of features after the trial ends.

If you're not sure where you stand, run [`dbt license info`](#troubleshooting) to check your status.

<!-- -->

## Usage[​](#usage "Direct link to Usage")

This page lists the commands and output you can use with `dbt login`:

* [`dbt login`](#dbt-login)
* [`dbt login --help`](#dbt-login---help)
* [`dbt login status`](#dbt-login-status)
* [`dbt license info`](#dbt-license-info)
* [`dbt --help`](#dbt---help)

### dbt login[​](#dbt-login "Direct link to dbt login")

Run `dbt login` from your terminal to open browser-based authentication:

```shell
dbt login
```

The CLI opens your default browser and shows the authentication URL:

```text
Opening your browser to complete login...
→ https://us1.dbt.com/register

If you need to reset your password, complete the reset, then re-run dbt login to finish authenticating.
Waiting for authentication…
```

After you sign in or register, dbt saves your credentials to your local dbt configuration directory:

* macOS and Linux: `~/.dbt/`
* Windows: `C:\Users\[username]\.dbt\`

Refer to [Staying signed in](#staying-signed-in) for details on how long you stay signed in.

When authentication completes successfully, the CLI shows:

```text
Congratulations! You are now signed in.
```

When authentication fails, the CLI shows:

```text
Authentication failed. Re-run dbt login to try again.

[Placeholder for user-facing error message on failure reason]
```

dbt State and dbt login

If you have a dbt platform account, you can use the same account to authenticate with dbt State — no separate sign-in required. For full dbt State setup, refer to [Setting up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md).

### dbt login --help[​](#dbt-login---help "Direct link to dbt login --help")

```text
Usage: dbt login [OPTIONS] <COMMAND>

Commands:
  status          Show dbt authentication status
```

### dbt login status[​](#dbt-login-status "Direct link to dbt login status")

Run `dbt login status` to show whether dbt is authenticated:

```shell
dbt login status
```

If dbt is authenticated, the CLI shows the authentication method:

```text
Status: authenticated (via METHOD)
```

If dbt is not authenticated, the CLI shows:

```text
Status: unauthenticated
```

### dbt license info[​](#dbt-license-info "Direct link to dbt license info")

Fusion uses local licenses to cache your logged-in state and give you access to advanced features. Run `dbt license info` as an additional check to verify the status of the license used by Fusion. This is useful when an advanced feature isn't working and `dbt login status` tells you you're authenticated:

```shell
dbt license info
```

The output should look like this:

```shell
dbtf license info
...
status: valid
licensing_enabled: true
license_type: developer
features: compare, state-aware-orchestration, strict-static-analysis
issued_at: June 17, 2026 10:55 UTC
expires_at: June 18, 2026 10:55 UTC
```

Add `--json` for machine-readable output:

```shell
dbt license info --json
```

For how to interpret each status, refer to [Troubleshooting](#troubleshooting).

### dbt --help[​](#dbt---help "Direct link to dbt --help")

`dbt login` and `dbt license` appear in the `dbt --help` available commands list:

```text
Available Commands:
  login          Log in to dbt
  license         Manage your dbt license
```

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

If an advanced feature isn't working, or you're not sure whether you're still signed in, run `dbt license info` to check your access:

```shell
dbt license info
```

Add `--json` for machine-readable output:

```shell
dbt license info --json
```

The output shows your current status. Use the following table to interpret it:

| Status            | What it means                                      | What to do                                                                                                    |
| ----------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `valid`           | You're signed in and your features are available.  | Nothing — you're all set.                                                                                     |
| `trial_expired`   | Your 14-day dbt VS Code extension trial has ended. | [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a free dbt platform account. |
| `expired`         | Your access has expired.                           | Run `dbt login` to sign in again.                                                                             |
| `not_found`       | dbt couldn't find any sign-in for you.             | Run `dbt login`, or set a [service token](#authenticate-with-a-service-token) for orchestrated runs.          |
| `invalid`         | Your access failed validation.                     | Run `dbt login` to refresh it.                                                                                |
| `transient_error` | A temporary network or server issue.               | Retry — your cached access stays active in the meantime.                                                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

If you just registered a new account

If you created a new dbt platform account but haven't verified your email yet, dbt warns you on each run during a short grace period.

After the grace period ends, advanced features stop working until you verify. You can still use the vast majority of features like code error diagnostics and Jinja LSP go-to ref definition, [and more](https://docs.getdbt.com/docs/fusion/fusion-availability.md?version=1.11#dbt-vs-code-extension-features).

Check your inbox for the verification email, or [contact dbt Support](https://docs.getdbt.com/docs/dbt-support.md) if you need it resent.

## Related commands[​](#related-commands "Direct link to Related commands")

* [`dbt login status`](#dbt-login-status) — Shows your current dbt authentication status.
* [`dbt license info`](#dbt-license-info) — Shows your current access status and helps diagnose feature availability.
* [`dbt init`](https://docs.getdbt.com/reference/commands/init.md) — Use during first-time project setup; prompts you to run `dbt login` to unlock authenticated features.
* [`dbt deps`](https://docs.getdbt.com/reference/commands/deps.md) — Installs packages for a dbt project.
* [`dbt debug`](https://docs.getdbt.com/reference/commands/debug.md) — Tests your dbt project and connection configuration.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
