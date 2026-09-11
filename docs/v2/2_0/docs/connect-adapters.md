# Connect to adapters

Adapters are an essential component of dbt. At their most basic level, they are how dbt connects with the various supported data platforms. At a higher-level, adapters strive to give analytics engineers more transferrable skills as well as standardize how analytics projects are structured. Gone are the days where you have to learn a new language or flavor of SQL when you move to a new job that has a different data platform. That is the power of adapters in dbt — for more detail, refer to the [Build, test, document, and promote adapters](../guides/adapter-creation.md) guide.

This section provides more details on different ways you can connect dbt to an adapter, and explains what a maintainer is.

### Set up in dbt

Explore the fastest and most reliable way to deploy dbt by using the dbt platform, a hosted architecture that runs dbt across your organization. The dbt platform lets you seamlessly [connect](./platform/about-platform-setup.md) with a variety of [trusted](./supported-data-platforms.md) data platform providers directly in the dbt UI.

### Install self-hosted dbt

Install self-hosted dbt locally using the command line. dbt communicates with a number of different data platforms by using a dedicated adapter plugin for each. When you install dbt locally, you'll also need to configure the specific adapter for your database, [connect dbt v2 to dbt v1](./local/install-dbt.md), and set up a `profiles.yml` file.

(Applies to dbt v2.0 and later)

Trusted adapters ship with dbt v2 — when you [install dbt](./local/install-dbt.md), the supported data platforms are available out of the box, with no separate `pip install` per adapter needed.

Refer to [adapter creation](../guides/adapter-creation-v2.md?step=1) for more info.

## Footnotes

1. Use the PyPI package name when installing with `pip`

   | Adapter repo name | PyPI package name    |
   | ----------------- | -------------------- |
   | `dbt-layer`       | `dbt-layer-bigquery` |

   [↩](#user-content-fnref-1)
