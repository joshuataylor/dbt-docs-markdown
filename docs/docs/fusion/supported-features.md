# Supported features

Learn about the features supported by the dbt Fusion engine, including requirements and limitations.

<!-- -->

<!-- -->

## Requirements[​](#requirements "Direct link to Requirements")

To use Fusion in your dbt project you must:

* Use a supported adapter and authentication method:

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

* Be able to run your project on the latest version of dbt Core with no deprecation warnings or errors.

* Migrate your Semantic Layer configurations to the [latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

## Parity with dbt Core[​](#parity-with-dbt-core "Direct link to Parity with dbt Core")

Our goal is for the dbt Fusion engine to support all capabilities of the dbt Core framework, and then some. Fusion already supports many of the capabilities in dbt Core v1.9, and we're working fast to add more.

Note that we have removed some deprecated features and introduced more rigorous validation of erroneous project code. Refer to the [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md) for details.

## Features and capabilities[​](#features-and-capabilities "Direct link to Features and capabilities")

dbt Fusion engine (built on Rust) gives your team up to 30x faster performance and comes with different features depending on where you use it.

* It powers both *engine-level* improvements (like faster compilation and incremental builds) and *editor-level* features (like IntelliSense, hover info, and inline errors) through the LSP through the dbt VS Code extension.
* To learn about the LSP features supported across the dbt platform, refer to [About dbt LSP](https://docs.getdbt.com/docs/about-dbt-lsp.md).
* To stay up-to-date on the latest features and capabilities, check out the [Fusion diaries](https://github.com/dbt-labs/dbt-fusion/discussions).

dbt Core (built on Python) supports SQL rendering but lacks SQL parsing and modern editor features powered by dbt Fusion engine and the LSP.

tip

dbt platform customers using Fusion can [develop across multiple development surfaces](https://docs.getdbt.com/docs/fusion/fusion-availability.md), including Studio IDE and VS Code with the dbt extension.

dbt platform [features](https://docs.getdbt.com/docs/cloud/about-cloud/dbt-cloud-features.md) (like [Advanced CI](https://docs.getdbt.com/docs/deploy/advanced-ci.md), [dbt Mesh](https://docs.getdbt.com/docs/mesh/about-mesh.md), [State-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md), and more) are available regardless of which surface you use, depending on your [dbt plan](https://www.getdbt.com/pricing).

If you're not sure what features are available in Fusion, the dbt VS Code extension, Fusion-CLI, or more, the following table focuses on Fusion-powered options.

In this table, self-hosted means it's open-source/source-available and runs on your own infrastructure; dbt platform is hosted by dbt Labs and includes platform-level features.

> ✅ = Available | 🟡 = Partial/at compile-time only | ❌ = Not available | Coming soon = Not yet available

| **Category/Capability**                         | **Fusion CLI**<br />(self-hosted) | **Fusion + VS Code extension**<br />(self-hosted) | **dbt platform**<br />\*\* + VS Code extension\*\*1 | **dbt platform** \*\* + Studio IDE\*\*<br />\*\* + Other dev surfaces\*\*2 | **Requires<br />[static analysis](https://docs.getdbt.com/docs/fusion/new-concepts.md#principles-of-static-analysis)** |
| ----------------------------------------------- | --------------------------------- | ------------------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Engine performance**                          |                                   |                                                   |                                                     |                                                                            |                                                                                                                        |
| SQL rendering                                   | ✅                                | ✅                                                | ✅                                                  | ✅                                                                         | ❌                                                                                                                     |
| SQL parsing and compilation (SQL understanding) | ✅                                | ✅                                                | ✅                                                  | ✅                                                                         | ✅                                                                                                                     |
| **Editor and dev experience**                   |                                   |                                                   |                                                     |                                                                            |                                                                                                                        |
| IntelliSense/autocomplete/hover info            | ❌                                | ✅                                                | ✅                                                  | ✅                                                                         | ✅                                                                                                                     |
| Inline errors (on save/in editor)               | 🟡                                | ✅                                                | ✅                                                  | ✅                                                                         | ✅                                                                                                                     |
| Live CTE previews/compiled SQL view             | ❌                                | ✅                                                | ✅                                                  | ✅                                                                         | 🟡<br />(Live CTE previews only)                                                                                       |
| Refactoring tools (rename model/column)         | ❌                                | ✅                                                | ✅                                                  | Coming soon                                                                | 🟡<br />(Column refactor only)                                                                                         |
| Go-to definition/references/macro               | ❌                                | ✅                                                | ✅                                                  | ✅                                                                         | 🟡<br />(Column go-to definition only)                                                                                 |
| Column-level lineage (in editor)                | ❌                                | ✅                                                | ✅                                                  | Coming soon                                                                | ✅                                                                                                                     |
| Developer compare changes                       | ❌                                | ❌                                                | Coming soon                                         | Coming soon                                                                | ❌                                                                                                                     |
| **Platform and governance**                     |                                   |                                                   |                                                     |                                                                            |                                                                                                                        |
| Advanced CI compare changes                     | ❌                                | ❌                                                | ✅                                                  | ✅                                                                         | ❌                                                                                                                     |
| dbt Mesh                                        | ❌                                | ❌                                                | ✅                                                  | ✅                                                                         | ❌                                                                                                                     |
| Efficient testing                               | ❌                                | ❌                                                | ✅                                                  | ✅                                                                         | ✅                                                                                                                     |
| State-aware orchestration (SAO)                 | ❌                                | ❌                                                | ✅                                                  | ✅                                                                         | ❌                                                                                                                     |
| Governance (PII/PHI tracking)                   | ❌                                | ❌                                                | Coming soon                                         | Coming soon                                                                | ✅                                                                                                                     |
| CI/CD cost optimization (Slimmer CI)            | ❌                                | ❌                                                | Coming soon                                         | Coming soon                                                                | ✅                                                                                                                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

1 Support for other dbt platform and LSP features, like Column-level lineage, is coming soon. See [About LSP](https://docs.getdbt.com/docs/about-dbt-lsp.md) for a more detailed comparison of dbt development environments.<br />2 The [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) is usable in VS Code, Cursor, Windsurf, and other VS Code–based editors.

#### Additional considerations[​](#additional-considerations "Direct link to Additional considerations")

Here are some additional considerations if using the Fusion CLI without the VS Code extension or the VS Code extension without the Fusion CLI:

* **Fusion CLI** ([binary](https://docs.getdbt.com/blog/dbt-fusion-engine-components))

  <!-- -->

  * Free to use and runs on the dbt Fusion engine (distinct from dbt Core).
  * Benefits from Fusion engine's performance for `parse`, `compile`, `build`, and `run`, but *doesn't* include LSP [features](https://docs.getdbt.com/docs/dbt-extension-features.md) like autocomplete, hover insights, lineage, and more.
  * Requires `profiles.yml` only (no `dbt_cloud.yml`).

* **dbt VS Code extension**

  * Free to use and runs on the dbt Fusion engine; register your email within 14 days.
  * Benefits from Fusion engine's performance for `parse`, `compile`, `build`, and `run`, and includes LSP [features](https://docs.getdbt.com/docs/dbt-extension-features.md) like autocomplete, hover insights, lineage, and more.
  * Capped at 15 users per organization. See the [acceptable use policy](https://www.getdbt.com/dbt-assets/vscode-plugin-aup) for more information.
  * If you already have a dbt platform user account (even if a trial expired), sign in with the same email. Unlock or reset it if locked.
  * Requires both `profiles.yml` and `dbt_cloud.yml` files.

## Limitations[​](#limitations "Direct link to Limitations")

If your project is using any of the features listed in the following table, you can use Fusion, but you won't be able to fully migrate all your workloads because you have:

* Models that leverage specific materialization features may be unable to run or may be missing some desirable configurations.
* Tooling that expects dbt Core's exact log output. Fusion's logging system is currently unstable and incomplete.
* Workflows built around complementary features of the dbt platform (like model-level notifications and Advanced CI) that Fusion does not yet support.
* When using the dbt VS Code extension in Cursor, lineage visualization works best in Editor mode and doesn't render in Agent mode. If you're working in Agent mode and need to view lineage, switch to Editor mode to access the full lineage tab functionality.

note

We have been moving quickly to implement many of these features ahead of General Availability. Read more about [the path to GA](https://docs.getdbt.com/blog/dbt-fusion-engine-path-to-ga), and track our progress in the [`dbt-fusion` milestones](https://github.com/dbt-labs/dbt-fusion/milestones).

<!-- -->

| Feature                                                                                                                                                                                                                          | This will affect you if...                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | GitHub issue                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| [--fail-fast](https://docs.getdbt.com/reference/global-configs/failing-fast.md)                                                                                                                                                  | You use the --fail-fast flag to interrupt runs at the first sign of failure.                                                                                                                                                                                                                                                                                                                                                                                                                 | [dbt-fusion#18](https://github.com/dbt-labs/dbt-fusion/issues/18)     |
| [microbatch incremental strategy](https://docs.getdbt.com/docs/build/incremental-microbatch.md)                                                                                                                                  | You are configuring models with materializations other than view, table, or incremental. You cannot yet run those models with Fusion, but you can run models using the standard materializations.                                                                                                                                                                                                                                                                                            | [dbt-fusion#12](https://github.com/dbt-labs/dbt-fusion/issues/12)     |
| [--warn-error, --warn-error-options](https://docs.getdbt.com/reference/global-configs/warnings.md)                                                                                                                               | You are upgrading all/specific warnings to errors, or silencing specific warnings, by configuring the warning event names. Fusion's logging system is incomplete and unstable, and so specific event names are likely to change.                                                                                                                                                                                                                                                             | [dbt-fusion#8](https://github.com/dbt-labs/dbt-fusion/issues/8)       |
| Iceberg support (BigQuery)                                                                                                                                                                                                       | You have configured models to be materialized as Iceberg tables, or you are defining `catalogs` in your BigQuery project to configure the external write location of Iceberg models. Fusion doesn't support these model configurations for BigQuery.                                                                                                                                                                                                                                         | [dbt-fusion#947](https://github.com/dbt-labs/dbt-fusion/issues/947)   |
| [Model-level notifications](https://docs.getdbt.com/docs/deploy/model-notifications.md)                                                                                                                                          | You are leveraging the dbt platform’s capabilities for model-level notifications in your workflows. Fusion currently supports job-level notifications.                                                                                                                                                                                                                                                                                                                                       | [dbt-fusion#1103](https://github.com/dbt-labs/dbt-fusion/issues/1103) |
| [retry](https://docs.getdbt.com/reference/commands/retry.md)                                                                                                                                                                     | Fusion does not yet support the dbt retry CLI command, or "rerun failed job from point of failure." In deployment environments, using [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md), you can simply rerun the job and Fusion will skip models that do not have fresh data or have not met their freshness.build\_after threshold since the last build                                                                                                | [dbt-fusion#21](https://github.com/dbt-labs/dbt-fusion/issues/21)     |
| [dbt-docs documentation site](https://docs.getdbt.com/docs/build/view-documentation.md#dbt-docs) and ["docs generate/serve" commands](https://docs.getdbt.com/reference/commands/cmd-docs.md)                                    | Fusion does not yet support a local experience for generating, hosting, and viewing documentation, as dbt Core does via dbt-docs (static HTML site). We intend to support such an experience by GA. If you need to generate and host local documentation, you should continue generating the catalog by running dbt docs generate with dbt Core.                                                                                                                                             | [dbt-fusion#9](https://github.com/dbt-labs/dbt-fusion/issues/9)       |
| [Programmatic invocations](https://docs.getdbt.com/reference/programmatic-invocations.md)                                                                                                                                        | You use dbt Core’s Python API for triggering invocations and registering callbacks on events/logs. Note that Fusion’s logging system is incomplete and unstable.                                                                                                                                                                                                                                                                                                                             | [dbt-fusion#10](https://github.com/dbt-labs/dbt-fusion/issues/10)     |
| [Linting via SQLFluff](https://docs.getdbt.com/docs/deploy/continuous-integration.md#to-configure-sqlfluff-linting)                                                                                                              | You use SQLFluff for linting in your development or CI workflows. Eventually, we plan to build linting support into Fusion directly, since the engine has SQL comprehension capabilities. In the meantime, you can continue using the dbt Core + SQLFluff integration. dbt Cloud will do exactly this in the Cloud IDE / Studio + CI jobs.                                                                                                                                                   | [dbt-fusion#11](https://github.com/dbt-labs/dbt-fusion/issues/11)     |
| [`{{ graph }}`](https://docs.getdbt.com/reference/dbt-jinja-functions/graph.md) - `raw_sql` attribute (for example, specific models in [dbt\_project\_evaluator](https://hub.getdbt.com/dbt-labs/dbt_project_evaluator/latest/)) | You access the `raw_sql` / `raw_code` attribute of the `{{ graph }}` context variable, which Fusion stubs with an empty value at runtime. If you access this attribute, your code will not fail, but it will return different results. This is used in three quality checks within the [`dbt_project_evaluator` package](https://hub.getdbt.com/dbt-labs/dbt_project_evaluator/latest/). We intend to find a more-performant mechanism for Fusion to provide this information in the future. | Coming soon                                                           |

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

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md)
* [Fusion licensing](http://www.getdbt.com/licenses-faq)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
