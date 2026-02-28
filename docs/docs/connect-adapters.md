# Connect to adapters

Adapters are an essential component of dbt. At their most basic level, they are how dbt connects with the various supported data platforms. At a higher-level, adapters strive to give analytics engineers more transferrable skills as well as standardize how analytics projects are structured. Gone are the days where you have to learn a new language or flavor of SQL when you move to a new job that has a different data platform. That is the power of adapters in dbt — for more detail, refer to the [Build, test, document, and promote adapters](https://docs.getdbt.com/guides/adapter-creation.md) guide.

This section provides more details on different ways you can connect dbt to an adapter, and explains what a maintainer is.

### Set up in dbt[​](#set-up-in-dbt "Direct link to Set up in dbt")

Explore the fastest and most reliable way to deploy dbt using dbt, a hosted architecture that runs dbt Core across your organization. dbt lets you seamlessly [connect](https://docs.getdbt.com/docs/cloud/about-cloud-setup.md) with a variety of [trusted](https://docs.getdbt.com/docs/supported-data-platforms.md) data platform providers directly in the dbt UI.

### Install with dbt Core[​](#install-with-dbt-core "Direct link to Install with dbt Core")

Install dbt Core, an open-source tool, locally using the command line. dbt communicates with a number of different data platforms by using a dedicated adapter plugin for each. When you install dbt Core, you'll also need to install the specific adapter for your database, [connect the dbt Fusion engine to dbt Core](https://docs.getdbt.com/docs/about-dbt-install.md), and set up a `profiles.yml` file.

With a few exceptions [1](#user-content-fn-1), you can install all [adapters](https://docs.getdbt.com/docs/supported-data-platforms.md) from PyPI using `python -m pip install adapter-name`. For example to install Snowflake, use the command `python -m pip install dbt-snowflake`. The installation will include `dbt-core` and any other required dependencies, which may include both other dependencies and even other adapter plugins. Read more about [installing dbt](https://docs.getdbt.com/docs/core/installation-overview.md).

<!-- -->

## Footnotes[​](#footnote-label "Direct link to Footnotes")

1. Use the PyPI package name when installing with `pip`

   | Adapter repo name | PyPI package name    |
   | ----------------- | -------------------- |
   | `dbt-layer`       | `dbt-layer-bigquery` |

   Search table...

   |                  |   |   |   |   |
   | ---------------- | - | - | - | - |
   | Loading table... |   |   |   |   |

   [↩](#user-content-fnref-1)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
