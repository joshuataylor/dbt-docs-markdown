# Build your metrics

Use MetricFlow in dbt to centrally define your metrics. As a key component of the [Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md), MetricFlow is responsible for SQL query construction and defining specifications for dbt semantic models and metrics. It uses familiar constructs like semantic models and metrics to avoid duplicative coding, optimize your development workflow, ensure data governance for company metrics, and guarantee consistency for data consumers.

For a complete list of configuration options and property specs, see the [Semantic Layer reference](../../reference/semantic-layer-reference.md).

[![This diagram shows how the dbt Semantic Layer works with your data stack.](/img/docs/dbt-platform/semantic-layer/sl-concept.png?v=2 "This diagram shows how the dbt Semantic Layer works with your data stack.")](#)This diagram shows how the dbt Semantic Layer works with your data stack.

MetricFlow allows you to:

* Intuitively define metrics in your dbt project
* Develop from your preferred environment, whether that's the [dbt platform CLI](../platform/dbt-cli-installation.md), [Studio IDE](../platform/studio-ide/develop-in-studio.md), or [dbt Core](../local/install-dbt.md)
* Use [MetricFlow commands](./metricflow-commands.md) to query and test those metrics in your development environment
* Harness the true magic of the universal Semantic Layer and dynamically query these metrics in downstream tools (Available for dbt [Starter, Enterprise, or Enterprise+](https://www.getdbt.com/pricing/) accounts only).

[![](/img/icons/dbt-bit.svg)](../../reference/semantic-layer-reference.md)

#### [Semantic Layer reference](../../reference/semantic-layer-reference.md)

[Complete, exhaustive configuration reference for semantic models, metrics, and dimensions.](../../reference/semantic-layer-reference.md)

[![](/img/icons/dbt-bit.svg)](./latest-metrics-spec.md)

#### [Migrate to the latest YAML spec](./latest-metrics-spec.md)

[Learn how to migrate from the legacy metrics YAML spec to the latest metrics YAML spec.](./latest-metrics-spec.md)

[![](/img/icons/dbt-bit.svg)](../../guides/sl-snowflake-qs.md)

#### [Quickstart for the dbt Semantic Layer](../../guides/sl-snowflake-qs.md)

[Use this guide to build and define metrics, set up the dbt Semantic Layer, and query them using downstream tools.](../../guides/sl-snowflake-qs.md)

[![](/img/icons/dbt-bit.svg)](./about-metricflow.md)

#### [About MetricFlow](./about-metricflow.md)

[Understand MetricFlow's core concepts, how to use joins, how to save commonly used queries, and what commands are available.](./about-metricflow.md)

[![](/img/icons/dbt-bit.svg)](./semantic-models.md)

#### [Semantic model](./semantic-models.md)

[Use semantic models as the basis for defining data. They act as nodes in the semantic graph, with entities connecting them.](./semantic-models.md)

[![](/img/icons/dbt-bit.svg)](./metrics-overview.md)

#### [Metrics](./metrics-overview.md)

[Define metrics in your dbt project using different metric types in YAML files.](./metrics-overview.md)

[![](/img/icons/dbt-bit.svg)](./advanced-topics.md)

#### [Advanced topics](./advanced-topics.md)

[Learn about advanced topics for dbt Semantic Layer and MetricFlow, such as data modeling workflows, and more.](./advanced-topics.md)

[![](/img/icons/dbt-bit.svg)](../use-dbt-semantic-layer/dbt-sl.md)

#### [About the dbt Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md)

[Introducing the dbt Semantic Layer, the universal process that allows data teams to centrally define and query metrics](../use-dbt-semantic-layer/dbt-sl.md)

[![](/img/icons/dbt-bit.svg)](../platform-integrations/avail-sl-integrations.md)

#### [Available integrations](../platform-integrations/avail-sl-integrations.md)

[Discover the diverse range of partners that seamlessly integrate with the powerful dbt Semantic Layer, allowing you to query and unlock valuable insights from your data ecosystem.](../platform-integrations/avail-sl-integrations.md)

<br />

## Related docs

* [Quickstart guide with the Semantic Layer](../../guides/sl-snowflake-qs.md)
* [The Semantic Layer: what's next](https://www.getdbt.com/blog/dbt-semantic-layer-whats-next/) blog
* [Semantic Layer on-demand course](https://learn.getdbt.com/courses/semantic-layer)
* [Semantic Layer FAQs](../use-dbt-semantic-layer/sl-faqs.md)
