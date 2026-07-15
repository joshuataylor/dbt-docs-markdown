# About self-hosted Fusion installation [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

<!-- -->

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

Learn more about installing Fusion locally, along with important prerequisites, step-by-step installation instructions, troubleshooting common issues, and configuration guidance.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before installing Fusion, ensure that you:

* Have administrative privileges to install software on your local machine.

* Are comfortable using a command-line interface (Terminal on macOS/Linux, PowerShell on Windows).

* Use a supported data warehouse and authentication method and configure permissions as needed:

  <!-- -->

   BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth
  * External OAuth
    <!-- -->
    * [Workload Identity Federation](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
  * [Required permissions](https://docs.getdbt.com/docs/local/connect-data-platform/bigquery-setup.md#required-permissions)

   Databricks[Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth

   Redshift[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Username / Password
  * IAM profile

   Snowflake

  * Username / Password
  * Native OAuth
  * External OAuth
  * Key pair using a modern PKCS#8 method
  * MFA

   Apache Spark (Fusion CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Thrift

    <!-- -->

    * Simple Authentication and Security Layer (SASL) PLAIN
    * No SASL (NOSASL)

  * Livy

    <!-- -->

    * Basic authentication (username and password)
    * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
      <!-- -->
      * Supports authentication using single sign-on, service accounts, or user tokens

   DuckDB (Fusion CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  DuckDB does not require authentication — it runs locally on your machine.

* Use a supported operating system:

  <!-- -->

  * **macOS:** Supported on both Intel (x86-64) and Apple Silicon (ARM)
  * **Linux:** Supported on both x86-64 and ARM
  * **Windows:** Supported on x86-64; ARM support coming soon

## Getting started[​](#getting-started "Direct link to Getting started")

If you're ready to get started, choose one of the following options. To learn more about which tool is best for you, see the [Fusion availability](https://docs.getdbt.com/docs/fusion/fusion-availability.md) page.

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

#### [dbt VS Code Extension](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

[Learn how to connect to a data platform, integrate with secure authentication methods, and configure a sync with a git repo.](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

#### [dbt Fusion engine from the CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

[Learn how to install the dbt Fusion engine on the command line interface (CLI).](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

#### [dbt Fusion engine upgrade](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

[Learn how you can upgrade and leverage the speed and scale of the dbt Fusion engine](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
