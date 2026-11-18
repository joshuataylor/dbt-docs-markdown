# Available adapters

Adapters connect dbt to data platforms. They are maintained by dbt Labs, partners, and community members.

We welcome [contributions to an existing adapter](./contribute-dbt-adapters.md#contribute-to-a-pre-existing-adapter). Many adapters are maintained by volunteers, so be kind and help where you can.

(Applies to dbt v2.0 and later)

To contribute a v2 adapter, refer to [Contribute a dbt v2 adapter](../guides/adapter-creation-v2.md?step=1).

### Adapter designations

Every v2 adapter lives in the [`dbt-labs/dbt`](https://github.com/dbt-labs/dbt) repository and ships in the same dbt binary. Each adapter card shows a designation that tells you who maintains the adapter's warehouse-specific code:

* `first-party`: dbt Labs builds, tests, releases, and maintains the adapter, and triages its issues. dbt downloads the adapter's ADBC driver from the dbt Labs CDN automatically. First-party adapters move through lifecycle stages: alpha, beta, preview, and general availability (GA).
* `community-maintained`: A community member or the data platform vendor contributes the adapter through pull requests and maintains its warehouse-specific code. dbt Labs reviews and merges those pull requests and maintains the code that all adapters share. Community-maintained adapters don't have a lifecycle stage, so their pages show a `community-maintained` label instead. dbt Labs doesn't guarantee that its CDN hosts their ADBC driver.

A designation doesn't decide whether an adapter is available in the dbt platform. For more details, refer to the [adapter designations FAQ](#adapter-designations-faq).

![](/img/icons/apache-spark.svg)

first-party

#### Apache Spark

* [Install with dbt v2](./local/connect-data-platform/spark-setup.md)

dbt v2

![](/img/icons/bigquery.svg)

first-party

#### BigQuery

* [Set up in the dbt platform](./platform/connect-data-platform/connect-bigquery.md)
* [Install with dbt v2](./local/connect-data-platform/bigquery-setup.md)

dbt platformdbt v2

![](/img/icons/clickhouse.svg)

first-party

#### ClickHouse

* [Set up in the dbt platform](./platform/connect-data-platform/connect-clickhouse.md)
* [Install with dbt v2](./local/connect-data-platform/clickhouse-setup.md)

dbt platformdbt v2

![](/img/icons/databricks.svg)

first-party

#### Databricks

* [Set up in the dbt platform](./platform/connect-data-platform/connect-databricks.md)
* [Install with dbt v2](./local/connect-data-platform/databricks-setup.md)

dbt platformdbt v2

![](/img/icons/duckdb-seeklogo.svg)

first-party

#### DuckDB

* [Install with dbt v2](./local/connect-data-platform/duckdb-setup.md?version=2)

dbt v2

![](/img/icons/exasol.svg)

community-maintained

#### Exasol

Setup page for dbt v2 isn't available yet.

dbt v2

![](/img/icons/redshift.svg)

first-party

#### Redshift

* [Set up in the dbt platform](./platform/connect-data-platform/connect-redshift.md)
* [Install with dbt v2](./local/connect-data-platform/redshift-setup.md)

dbt platformdbt v2

![](/img/icons/salesforce.svg)

community-maintained

#### Salesforce Data 360

* [Install with dbt v2](./local/connect-data-platform/salesforce-data-cloud-setup.md)

dbt v2

![](/img/icons/snowflake.svg)

first-party

#### Snowflake

* [Set up in the dbt platform](./platform/connect-data-platform/connect-snowflake.md)
* [Install with dbt v2](./local/connect-data-platform/snowflake-setup.md)

dbt platformdbt v2

(Applies to dbt v2.0 and later)

### Adapter designations FAQ

Each v2 adapter on this page is labeled `first-party` or `community-maintained`. These questions explain what the labels mean for you as a user or a contributor.

 What's the difference between first-party and community-maintained adapters?

The difference is who maintains the adapter's warehouse-specific code. Both kinds live in the [`dbt-labs/dbt`](https://github.com/dbt-labs/dbt) repository and ship in the same dbt binary.

* `first-party`: dbt Labs builds, tests, releases, and maintains the adapter, and triages its issues.
* `community-maintained`: A community member or the data platform vendor contributes the adapter through pull requests and maintains its warehouse-specific code. dbt Labs reviews and merges those pull requests and maintains the code that all adapters share.

 Does the designation change how I install an adapter?

It depends on the designation. The dbt binary doesn't bundle ADBC drivers. For a first-party adapter, dbt downloads the driver from the dbt Labs CDN the first time you run a dbt command, so you don't install it yourself. For a community-maintained adapter, dbt Labs doesn't guarantee that its CDN hosts the driver. Check the adapter's setup guide to find out whether you need to install it yourself.

For the network access that driver downloads need, and how to install drivers in a restricted network, refer to [dbt v2 networking requirements](./local/dbt-networking-requirements.md?version=2#adapter-drivers).

 Do community-maintained adapters have a lifecycle stage?

No. Only first-party adapters move through lifecycle stages: alpha, beta, preview, and general availability (GA). A first-party adapter's setup page shows a label, such as Beta or Preview, until the adapter reaches GA.

Community-maintained adapters don't have a lifecycle stage. Their pages show a `community-maintained` label in place of a lifecycle label.

 Where do I report a problem with an adapter?

Open an issue in the [`dbt-labs/dbt` repository](https://github.com/dbt-labs/dbt/issues) for either designation. For a community-maintained adapter, the adapter's maintainers respond to issues about its warehouse-specific behavior, and dbt Labs handles issues in the shared code.

 Can I use a community-maintained adapter in the dbt platform?

The designation doesn't decide availability in the dbt platform. A **Set up in the dbt platform** link on the card shows where to look. Open that page and check which dbt version it supports.

dbt Labs is working toward a future where any v2 adapter, first-party or community-maintained, can be supported in the dbt platform. A v2 adapter with a working ADBC driver is the starting point.

 How do I get an adapter into the dbt platform?

dbt Labs is still defining this process. Today, the path looks like this:

1. Get an ADBC driver for your data platform. You need a working driver before anything else. Build one yourself, work with a driver specialist such as [Columnar](https://columnar.tech/), or combine both approaches. For where to look first, refer to [Contribute a dbt v2 adapter](../guides/adapter-creation-v2.md).
2. Contribute a v2 adapter. Open pull requests against [`dbt-labs/dbt`](https://github.com/dbt-labs/dbt) and get the adapter merged.
3. Ask for dbt platform support. Reach out through the [Fivetran contact page](https://www.fivetran.com/contact), or explore the [Fivetran Partner Program](https://www.fivetran.com/partners/become-a-partner) if you represent the data platform vendor. Requests are prioritized partly on business impact, so it helps to bring details such as your total addressable market (TAM), expected annual recurring revenue (ARR), and customer demand. The `dbt-labs/dbt` repository is for the open source code only, so don't open an issue there for dbt platform requests.

New dbt platform support is for v2 adapters only. If your adapter isn't in the dbt platform today, a v1 adapter alone isn't enough.

 If I contribute a new adapter, which designation does it get?

A new adapter that you contribute is `community-maintained`. You, or your organization, maintain its warehouse-specific code, and the dbt Labs adapters team reviews your pull requests. To get started, refer to [Contribute a dbt v2 adapter](../guides/adapter-creation-v2.md).
