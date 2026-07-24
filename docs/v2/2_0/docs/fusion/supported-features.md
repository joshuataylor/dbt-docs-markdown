# Supported features

Learn about the features supported by Fusion, including requirements and limitations.

<!-- -->

<!-- -->

When you install dbt, you get Fusion by default. There's no separate feature set to choose between — Fusion is just dbt, running faster, with more capability built in.

## Requirements[​](#requirements "Direct link to Requirements")

To use Fusion in your project you must:

* Use a supported adapter and authentication method:

  <!-- -->

   BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth
  * External OAuth
    <!-- -->
    * [Workload Identity Federation](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
  * [Required permissions](https://docs.getdbt.com/docs/local/connect-data-platform/bigquery-setup.md#required-permissions)

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

    <!-- -->

    * Simple Authentication and Security Layer (SASL) PLAIN
    * No SASL (NOSASL)

  * Livy

    <!-- -->

    * Basic authentication (username and password)
    * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
      <!-- -->
      * Supports authentication using single sign-on, service accounts, or user tokens

   DuckDB (CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  DuckDB does not require authentication — it runs locally on your machine.

  *Note that adapter lifecycle may differ between the dbt platform and local development. An adapter can reach GA in the dbt platform before it reaches GA for local use.*

* Be able to run your project on the latest version of dbt Core v1.x with no deprecation warnings or errors.

* Migrate your Semantic Layer configurations to the [latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

## Parity with dbt Core v1.x[​](#parity-with-dbt-core-v1x "Direct link to Parity with dbt Core v1.x")

Fusion supports nearly all of dbt Core v1.x's capabilities today. Refer to [Limitations](#limitations) below for the small number of gaps that remain.

Fusion has also removed some deprecated features and introduced more rigorous validation of erroneous project code compared to dbt Core v1.x. Refer to the [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) for details.

## Features and capabilities[​](#features-and-capabilities "Direct link to Features and capabilities")

Fusion gives your team faster development workflows with semantic and syntax error detection, a faster linter, column-level lineage, language server and VS Code integration, docs v2 (full), and data diff. The dbt VS Code extension adds editor features like IntelliSense, hover info, and inline errors on top, powered by the LSP.

Most Fusion features work right away, with no login required. A few more unlock once you sign in with a dbt platform account — free to create, no paid plan needed. For the full free-vs-login breakdown, refer to [Fusion availability](https://docs.getdbt.com/docs/fusion/fusion-availability.md). For LSP features specifically, refer to [About dbt LSP](https://docs.getdbt.com/docs/about-dbt-lsp.md). To stay up-to-date on the latest features, check out the [Fusion diaries](https://github.com/dbt-labs/dbt-fusion/discussions).

tip

dbt platform [features](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md) (like [Advanced CI](https://docs.getdbt.com/docs/deploy/advanced-ci.md), [dbt Mesh](https://docs.getdbt.com/docs/mesh/about-mesh.md), and more) are the enterprise layer on top of Fusion — available no matter how you run dbt, depending on your [dbt plan](https://www.getdbt.com/pricing).

## Limitations[​](#limitations "Direct link to Limitations")

If your project uses any of the following, you can still use Fusion, but full migration may not be possible yet:

* Models that rely on materialization features Fusion doesn't fully support, or that need configurations it's still missing
* Tooling that depends on dbt Core v1.x's exact log output — Fusion's logging system is still unstable and incomplete
* Workflows built around dbt platform features Fusion doesn't yet support, like model-level notifications
* Using the dbt VS Code extension in Cursor's Agent mode — lineage visualization only renders in Editor mode, so switch there if you need the full lineage tab

note

We have been moving quickly to implement many of these features ahead of General Availability. Read more about [the path to GA](https://docs.getdbt.com/blog/dbt-fusion-engine-path-to-ga), and track our progress in the [`dbt-fusion` milestones](https://github.com/dbt-labs/dbt-fusion/milestones).

<!-- -->

| Feature                                                                                                               | This will affect you if...                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | GitHub issue                                                      |
| --------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| [Programmatic invocations](https://docs.getdbt.com/reference/programmatic-invocations.md)                             | You use dbt Core’s Python API for triggering invocations and registering callbacks on events/logs. Note that Fusion’s logging system is a work in progress.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | [dbt-fusion#10](https://github.com/dbt-labs/dbt-fusion/issues/10) |
| [Linting using SQLFluff](https://docs.getdbt.com/docs/deploy/continuous-integration.md#to-configure-sqlfluff-linting) | You use SQLFluff for linting in CI or local development. SQLFluff is not natively compatible with the dbt Fusion engine but we provide a [SQLFluff compatible high performance alternative](https://docs.getdbt.com/reference/commands/lint.md?version=2.0) with Fusion. Support varies by where you run it:<br /><br />**dbt platform CI jobs**: SQLFluff linting is not available when running on Fusion.<br />**Studio IDE**: SQLFluff linting works, but uses the dbt Core engine templater rather than Fusion.<br />**Local development**: You can run SQLFluff locally using the standalone dbt Core engine templater as a workaround. A native Fusion linter is available with [`dbt lint` command](https://docs.getdbt.com/reference/commands/lint.md?version=2.0) | [dbt-fusion#11](https://github.com/dbt-labs/dbt-fusion/issues/11) |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Package support[​](#package-support "Direct link to Package support")

<!-- -->

To determine if a package is compatible with the dbt Fusion engine, visit the [dbt package hub](https://hub.getdbt.com/) and look for the Fusion-compatible badge, or review the package's [`require-dbt-version` configuration](https://docs.getdbt.com/reference/project-configs/require-dbt-version.md#pin-to-a-range).

* Packages with a `require-dbt-version` that equals or contains `2.0.0` are compatible with Fusion. For example, `require-dbt-version: ">=1.10.0,<3.0.0"`.

  Even if a package doesn't reflect compatibility in the package hub, it may still work with Fusion. Work with package maintainers to track updates, and [thoroughly test packages](https://docs.getdbt.com/guides/fusion-package-compat?step=5) that aren't clearly compatible before deploying.

* Package maintainers who would like to make their package compatible with Fusion can refer to the [Fusion package upgrade guide](https://docs.getdbt.com/guides/fusion-package-compat.md) for instructions.

Fivetran package considerations:

* The Fivetran `source` and `transformation` packages have been combined into a single package.
* If you manually installed source packages like `fivetran/github_source`, you need to ensure `fivetran/github` is installed and deactivate the transformation models.

<!-- -->

#### Package compatibility messages[​](#package-compatibility-messages "Direct link to Package compatibility messages")

Inconsistent Fusion warnings and `dbt-autofix` logs

Fusion warnings and `dbt-autofix` logs may show different messages about package compatibility.

If you use [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) while upgrading to Fusion in the Studio IDE or dbt VS Code extension, you may see different messages about package compatibility between `dbt-autofix` and Fusion warnings.

Here's why:

* Fusion warnings are emitted based on a package's `require-dbt-version` and whether `require-dbt-version` contains `2.0.0`.
* Some packages are already Fusion-compatible even though package maintainers haven't yet updated `require-dbt-version`.
* `dbt-autofix` knows about these compatible packages and will not try to upgrade a package that it knows is already compatible.

This means that even if you see a Fusion warning for a package that `dbt-autofix` identifies as compatible, you don't need to change the package.

The message discrepancy is temporary while we implement and roll out `dbt-autofix`'s enhanced compatibility detection to Fusion warnings.

Here's an example of a Fusion warning in the Studio IDE that says a package isn't compatible with Fusion but `dbt-autofix` indicates it is compatible:

```text
dbt1065: Package 'dbt_utils' requires dbt version [>=1.30,<2.0.0], but current version is 2.0.0-preview.72. This package may not be compatible with your dbt version. dbt(1065) [Ln 1, Col 1]
```

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)
