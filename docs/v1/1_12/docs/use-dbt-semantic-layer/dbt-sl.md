# dbt Semantic Layer

dbt platform | Starter, Enterprise, Enterprise+

The dbt Semantic Layer eliminates duplicate coding by allowing data teams to define metrics on top of existing models and automatically handling data joins.

The dbt Semantic Layer, powered by [MetricFlow](../build/about-metricflow.md), simplifies the process of defining and using critical business metrics, like `revenue` in the modeling layer (your dbt project). By centralizing metric definitions, data teams can ensure consistent self-service access to these metrics in downstream data tools and applications.

Moving metric definitions out of the BI layer and into the modeling layer allows data teams to feel confident that different business units are working from the same metric definitions, regardless of their tool of choice. If a metric definition changes in dbt, it’s refreshed everywhere it’s invoked and creates consistency across all applications. To ensure secure access control, the Semantic Layer implements robust [access permissions](./setup-sl.md#set-up-dbt-semantic-layer) mechanisms.

Refer to the [Semantic Layer FAQs](./sl-faqs.md) or [Why we need a universal semantic layer](https://www.getdbt.com/blog/universal-semantic-layer/) blog post to learn more.

[YouTube video player](https://www.youtube.com/embed/DS7Ub_CmBR0?si=m92hLmxw1VuE6KKO)

## Get started with the dbt Semantic Layer

To define and query metrics with the dbt Semantic Layer, you must be on a [dbt Starter or Enterprise-tier](https://www.getdbt.com/pricing/) account. Suitable for both Multi-tenant and Single-tenant accounts. Note: Single-tenant accounts should contact their account representative for necessary setup and enablement.

This page points to various resources available to help you understand, configure, deploy, and integrate the Semantic Layer. The following sections contain links to specific pages that explain each aspect in detail. Use these links to navigate directly to the information you need, whether you're setting up the Semantic Layer for the first time, deploying metrics, or integrating with downstream tools.

Refer to the following resources to get started with the Semantic Layer:

* [Quickstart with the Semantic Layer](../../guides/sl-snowflake-qs.md) — Build and define metrics, set up the Semantic Layer, and query them using our first-class integrations.
* [Build your metrics](../build/build-metrics-intro.md) — Use MetricFlow in dbt to centrally define your metrics.
* [Semantic Layer FAQs](./sl-faqs.md) — Discover answers to frequently asked questions about the Semantic Layer, such as availability, integrations, and more.

## Configure the dbt Semantic Layer

The following resources provide information on how to configure the Semantic Layer:

* [Administer the Semantic Layer](./setup-sl.md) — Seamlessly set up the credentials and tokens to start querying the Semantic Layer.
* [Architecture](./sl-architecture.md) — Explore the powerful components that make up the Semantic Layer.

## Deploy metrics

This section provides information on how to deploy the Semantic Layer and materialize your metrics:

* [Deploy your Semantic Layer](./deploy-sl.md) — Run a dbt job to deploy the Semantic Layer and materialize your metrics.
* [Write queries with exports](./exports.md) — Use exports to write commonly used queries directly within your data platform, on a schedule.
* [Cache common queries](./sl-cache.md) — Leverage result caching and declarative caching for common queries to speed up performance and reduce query computation.

## Consume metrics and integrate

Consume metrics and integrate the Semantic Layer with downstream tools and applications:

* [Consume metrics](./consume-metrics.md) — Query and consume metrics in downstream tools and applications using the Semantic Layer.
* [Available integrations](../platform-integrations/avail-sl-integrations.md) — Review a wide range of partners you can integrate and query with the Semantic Layer.
* [Semantic Layer APIs](../dbt-apis/sl-api-overview.md) — Use the Semantic Layer APIs to query metrics in downstream tools for consistent, reliable data metrics.
