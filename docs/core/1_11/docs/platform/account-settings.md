# Account settings in dbt

The following sections describe the different **Account settings** available from your dbt account in the sidebar (under your account name on the lower left-hand side).

[![Example of Account settings from the sidebar](/img/docs/dbt-platform/example-sidebar-account-settings.png?v=2 "Example of Account settings from the sidebar")](#)Example of Account settings from the sidebar

## Git repository caching [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#git-repository-caching- "Direct link to git-repository-caching-")

repo caching enabled by default

Git repository caching is enabled by default for all new Enterprise and Enterprise+ accounts, improving reliability by allowing dbt to use a cached copy of your repo if cloning fails.

See the next section for more details on repo caching, retention, and more.

At the start of every [job](https://docs.getdbt.com/docs/deploy/jobs.md) run, dbt clones the project's Git repository so it has the latest versions of your project's code and runs `dbt deps` to install your dependencies.

For improved reliability and performance on your job runs, you can enable dbt to keep a cache of the project's Git repository. So, if there's a third-party outage that causes the cloning operation to fail, dbt will instead use the cached copy of the repo so your jobs can continue running as scheduled.

dbt caches your project's Git repo after each successful run and retains it for 8 days if there are no repo updates. It caches all packages regardless of installation method and does not fetch code outside of the job runs.

dbt will use the cached copy of your project's Git repo under these circumstances:

* Outages from third-party services (for example, the [dbt package hub](https://hub.getdbt.com/)).
* Git authentication fails.
* There are syntax errors in the `packages.yml` file. You can set up and use [continuous integration (CI)](https://docs.getdbt.com/docs/deploy/continuous-integration.md) to find these errors sooner.
* If a package doesn't work with the current dbt version. You can set up and use [continuous integration (CI)](https://docs.getdbt.com/docs/deploy/continuous-integration.md) to identify this issue sooner.
* Note, Git repository caching should not be used for CI jobs. CI jobs are designed to test the latest code changes in a pull request and ensure your code is up to date. Using a cached copy of the repo in CI jobs could result in stale code being tested.

To use, select the **Enable repository caching** option from your account settings.

[![Example of the Enable repository caching option](/img/docs/deploy/account-settings-repository-caching.png?v=2 "Example of the Enable repository caching option")](#)Example of the Enable repository caching option

## Partial parsing[​](#partial-parsing "Direct link to Partial parsing")

At the start of every dbt invocation, dbt reads all the files in your project, extracts information, and constructs an internal manifest containing every object (model, source, macro, and so on). Among other things, it uses the `ref()`, `source()`, and `config()` macro calls within models to set properties, infer dependencies, and construct your project's DAG. When dbt finishes parsing your project, it stores the internal manifest in a file called `partial_parse.msgpack`.

Parsing projects can be time-consuming, especially for large projects with hundreds of models and thousands of files. To reduce the time it takes dbt to parse your project, use the partial parsing feature in dbt for your environment. When enabled, dbt uses the `partial_parse.msgpack` file to determine which files have changed (if any) since the project was last parsed, and then it parses *only* the changed files and the files related to those changes.

Partial parsing in dbt requires dbt version 1.4 or newer. The feature does have some known limitations. Refer to [Known limitations](https://docs.getdbt.com/reference/parsing.md#known-limitations) to learn more about them.

To use, select the **Enable partial parsing between deployment runs** option from your account settings.

[![Example of the Enable partial parsing between deployment runs option](/img/docs/deploy/account-settings-partial-parsing.png?v=2 "Example of the Enable partial parsing between deployment runs option")](#)Example of the Enable partial parsing between deployment runs option

## Account access and enablement[​](#account-access-and-enablement "Direct link to Account access and enablement")

Enable access to features in your account by selecting the appropriate option from your account settings.

### Enabling AI features[​](#enabling-ai-features "Direct link to Enabling AI features")

Admins can enable access to both dbt Wizard (dbt Labs’ AI agent layer, available on the dbt platform and CLI) and dbt Copilot for authorized users across the dbt platform.

### Enabling Advanced CI features [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#enabling-advanced-ci-features- "Direct link to enabling-advanced-ci-features-")

[Advanced CI](https://docs.getdbt.com/docs/deploy/advanced-ci.md) features, such as [compare changes](https://docs.getdbt.com/docs/deploy/advanced-ci.md#compare-changes), allow dbt account members to view details about the changes between what's in the production environment and the pull request.

To use Advanced CI features, your dbt account must have access to them. Ask your dbt administrator to enable Advanced CI features on your account, which they can do by choosing the **Enable account access to Advanced CI** option from the account settings.

Once enabled, the **dbt compare** option becomes available in the CI job settings for you to select.

[![The Enable account access to Advanced CI option](/img/docs/deploy/account-settings-advanced-ci.png?v=2 "The Enable account access to Advanced CI option")](#)The Enable account access to Advanced CI option

### Enabling external metadata ingestion in dbt Catalog [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#enabling-external-metadata-ingestion-in-dbt-catalog- "Direct link to enabling-external-metadata-ingestion-in-dbt-catalog-")

[Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) allows you to view your project's resources (for example, models, tests, and metrics), their lineage, and model consumption to gain a better understanding of your project's latest production state.

You can bring [external metadata](https://docs.getdbt.com/docs/explore/external-metadata-ingestion.md) into Catalog by connecting directly to your warehouse. This enables you to view tables and other assets that aren't defined in dbt. Currently, external metadata ingestion is supported for Snowflake only.

To use external metadata ingestion, you must be an [account admin](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#account-admin) with permission to edit connections. Enable Catalog in your account by selecting the **Ingest external metadata in dbt Catalog (formerly dbt Explorer)** option from your account settings. For more information, see [Enable external metadata ingestion](https://docs.getdbt.com/docs/explore/external-metadata-ingestion.md#enable-external-metadata-ingestion).

### Enable global account discovery[​](#enable-global-account-discovery "Direct link to Enable global account discovery")

When **Enable global account discovery** is on, users can discover all accounts associated with their email address at login. Users still access accounts using their credentials or the account's designated auth method (for example, SSO). Refer to [Log in to dbt platform](https://docs.getdbt.com/docs/platform/about-platform/login.md) for more info.

<!-- -->

info

`login.dbt.com` is currently available for multi-tenant accounts with an account-specific domain (for example, `abc123.us1.dbt.com`). Support for single-tenant accounts is coming soon. In the meantime, single-tenant users can sign in directly using their account **Access URL** (like `MY_COMPANY.us1.dbt.com`).

OAuth clients such as [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md), the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md?version=2.0), and [dbt MCP](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md) have not yet been updated to use `login.dbt.com` and continue to authenticate through their account [**Access URL**](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

Disabling this setting means users must know their [account URL](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md#accessing-your-account) to log in; they will not see a list of accounts at login.

To change this setting, select or clear the **Enable global account discovery** option in your account settings. If you disable it, a confirmation pop-up box explains that users will need the account URL to log in and access the account.

## Project settings history[​](#project-settings-history "Direct link to Project settings history")

You can view historical project settings changes over the last 90 days.

To view the change history:

1. Click your account name at the bottom of the left-side menu and click **Account settings**.
2. Click **Projects**.
3. Click a **project name**.
4. Click **History**.

[![Example of the project history option. ](/img/docs/deploy/project-history.png?v=2 "Example of the project history option. ")](#)Example of the project history option.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
