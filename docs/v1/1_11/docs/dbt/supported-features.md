# Supported features

Learn about the features supported by dbt v2, including requirements and limitations.

(Applies to dbt v1.99 and earlier)

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](../local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](../install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](../dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

When you install dbt, you get v2 by default. There's no separate feature set to choose between — v2 is just dbt, running faster, with more capability built in.

## Requirements

To use v2 in your project you must:

* Use a supported adapter and authentication method:

   BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth
  * External OAuth
    * [Workload Identity Federation](../platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
  * [Required permissions](../local/connect-data-platform/bigquery-setup.md#required-permissions)

   Databricks[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

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

   Apache Spark (CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Thrift

    * Simple Authentication and Security Layer (SASL) PLAIN
    * No SASL (NOSASL)

  * Livy

    * Basic authentication (username and password)
    * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
      * Supports authentication using single sign-on, service accounts, or user tokens

   DuckDB (CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  DuckDB does not require authentication — it runs locally on your machine.

  *Note that adapter lifecycle may differ between the dbt platform and local development. An adapter can reach GA in the dbt platform before it reaches GA for local use.*

* Be able to run your project on the latest version of dbt Core v1.x with no deprecation warnings or errors.

* Migrate your Semantic Layer configurations to the [latest YAML spec](../build/latest-metrics-spec.md).

## Parity with dbt Core v1.x

dbt v2 supports nearly all of v1.x's capabilities today. Refer to [Limitations](#limitations) below for the small number of gaps that remain.

v2 has also removed some deprecated features and introduced more rigorous validation of erroneous project code compared to v1.x. Refer to the [Upgrade guide](../dbt-versions/dbt-upgrade/upgrading-to-v2.md) for details.

## Features and capabilities

dbt v2 gives your team faster development workflows with semantic and syntax error detection, a faster linter, column-level lineage, language server and VS Code integration, docs v2 (full), and data diff. The dbt VS Code extension adds editor features like IntelliSense, hover info, and inline errors on top, powered by the LSP.

Most v2 features work right away, with no login required. A few more unlock once you sign in with a dbt platform account — free to create, no paid plan needed. For the full free-vs-login breakdown, refer to [v2 feature availability](./dbt-availability.md). For LSP features specifically, refer to [About dbt LSP](../about-dbt-lsp.md).

tip

dbt platform [features](../platform/about-platform/dbt-platform-features.md) (like [Advanced CI](../deploy/advanced-ci.md), [dbt Mesh](../mesh/about-mesh.md), and more) are the enterprise layer on top of v2 — available no matter how you run dbt, depending on your [dbt plan](https://www.getdbt.com/pricing).

## Limitations

If your project uses any of the following, you can still use dbt v2, but full migration may not be possible yet:

* Models that rely on materialization features v2 doesn't fully support, or that need configurations it's still missing
* Tooling that depends on v1.x's exact log output — v2's logging system is still unstable and incomplete
* Workflows built around dbt platform features v2 doesn't yet support, like model-level notifications
* Using the dbt VS Code extension in Cursor's Agent mode — lineage visualization only renders in Editor mode, so switch there if you need the full lineage tab

| Feature                                                                                                               | This will affect you if...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | GitHub issue                                                      |
| --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| [Programmatic invocations](../../reference/programmatic-invocations.md)                             | You use dbt Core’s Python API for triggering invocations and registering callbacks on events/logs. Note that Fusion’s logging system is a work in progress.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | [dbt-fusion#10](https://github.com/dbt-labs/dbt-fusion/issues/10) |
| [Linting using SQLFluff](../deploy/continuous-integration.md#to-configure-sqlfluff-linting) | You use SQLFluff for linting in CI or local development. SQLFluff is not natively compatible with the dbt Fusion engine but we provide a [SQLFluff compatible high performance alternative](../../reference/commands/lint.md?version=2.0) with Fusion. Support varies by where you run it:<br /><br />\*\*dbt platform **Studio IDE**: SQLFluff linting works, but uses the dbt Core engine templater rather than Fusion.<br />**Local development**: You can run SQLFluff locally using the standalone dbt Core engine templater as a workaround. A native Fusion linter is available with [`dbt lint` command](../../reference/commands/lint.md?version=2.0) | [dbt-fusion#11](https://github.com/dbt-labs/dbt-fusion/issues/11) |

## Package support

To determine if a package is compatible with dbt v2, visit the [dbt package hub](https://hub.getdbt.com/) and look for the Fusion-compatible badge, or review the package's [`require-dbt-version` configuration](../../reference/project-configs/require-dbt-version.md#pin-to-a-range).

* Packages with a `require-dbt-version` that equals or contains `2.0.0` are compatible with dbt v2. For example, `require-dbt-version: ">=1.10.0,<3.0.0"`.

  Even if a package doesn't reflect compatibility in the package hub, it may still work with v2. Work with package maintainers to track updates, and [thoroughly test packages](https://docs.getdbt.com/guides/dbt-package-compat?step=5) that aren't clearly compatible before deploying.

* Package maintainers who would like to make their package compatible with v2 can refer to the [dbt v2 package upgrade guide](../../guides/dbt-package-compat.md) for instructions.

Fivetran package considerations:

* The Fivetran `source` and `transformation` packages have been combined into a single package.
* If you manually installed source packages like `fivetran/github_source`, you need to ensure `fivetran/github` is installed and deactivate the transformation models.

#### Package compatibility messages

Inconsistent v2 warnings and `dbt-autofix` logs

dbt v2 warnings and `dbt-autofix` logs may show different messages about package compatibility.

If you use [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) while upgrading to v2 in the Studio IDE or dbt VS Code extension, you may see different messages about package compatibility between `dbt-autofix` and v2 warnings.

Here's why:

* dbt v2 warnings are emitted based on a package's `require-dbt-version` and whether `require-dbt-version` contains `2.0.0`.
* Some packages are already v2-compatible even though package maintainers haven't yet updated `require-dbt-version`.
* `dbt-autofix` knows about these compatible packages and will not try to upgrade a package that it knows is already compatible.

This means that even if you see a v2 warning for a package that `dbt-autofix` identifies as compatible, you don't need to change the package.

The message discrepancy is temporary while we implement and roll out `dbt-autofix`'s enhanced compatibility detection to v2 warnings.

Here's an example of a dbt v2 warning in the Studio IDE that says a package isn't compatible with v2 but `dbt-autofix` indicates it is compatible:

```text
dbt1065: Package 'dbt_utils' requires dbt version [>=1.30,<2.0.0], but current version is 2.0.0-preview.72. This package may not be compatible with your dbt version. dbt(1065) [Ln 1, Col 1]
```

## More information about dbt v2

* [About the dbt extension](../about-dbt-extension.md)
* [Supported features matrix](./supported-features.md)
* [Install dbt](../local/install-dbt.md)
* [Quickstart for Fusion](../../guides/dbt.md?step=1)
* [Upgrade guide](../dbt-versions/dbt-upgrade/upgrading-to-v2.md)
* [dbt v2 license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)
