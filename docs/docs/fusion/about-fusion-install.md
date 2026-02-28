# About Fusion local installation [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

<!-- -->

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine) [Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-fusion/discussions/categories/announcements) for the latest updates.

Learn more about installing Fusion locally, along with important prerequisites, step-by-step installation instructions, troubleshooting common issues, and configuration guidance.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before installing Fusion, ensure that you:

* Have administrative privileges to install software on your local machine.

* Are comfortable using a command-line interface (Terminal on macOS/Linux, PowerShell on Windows).

* Use a supported data warehouse and authentication method and configure permissions as needed:

  <!-- -->

   BigQuery

  * Service Account / User Token
  * Native OAuth
  * External OAuth
  * [Required permissions](https://docs.getdbt.com/docs/core/connect-data-platform/bigquery-setup.md#required-permissions)

   Databricks

  * Service Account / User Token
  * Native OAuth

   Redshift

  * Username / Password
  * IAM profile

   Snowflake

  * Username / Password
  * Native OAuth
  * External OAuth
  * Key pair using a modern PKCS#8 method
  * MFA

* Use a supported operating system:

  <!-- -->

  * **macOS:** Supported on both Intel (x86-64) and Apple Silicon (ARM)
  * **Linux:** Supported on both x86-64 and ARM
  * **Windows:** Supported on x86-64; ARM support coming soon

## Getting started[​](#getting-started "Direct link to Getting started")

If you're ready to get started, choose one of the following options. To learn more about which tool is best for you, see the [Fusion availability](https://docs.getdbt.com/docs/fusion/fusion-availability.md) page.

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/fusion/install-dbt-extension.md)

#### [dbt VS Code Extension](https://docs.getdbt.com/docs/fusion/install-dbt-extension.md)

[Learn how to connect to a data platform, integrate with secure authentication methods, and configure a sync with a git repo.](https://docs.getdbt.com/docs/fusion/install-dbt-extension.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)

#### [dbt Fusion engine from the CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)

[Learn how to install the dbt Fusion engine on the command line interface (CLI).](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)

#### [dbt Fusion engine upgrade](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)

[Learn how you can upgrade and leverage the speed and scale of the dbt Fusion engine](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
