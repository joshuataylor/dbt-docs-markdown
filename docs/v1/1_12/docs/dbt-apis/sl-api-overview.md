# Semantic Layer APIs

dbt platform | Starter, Enterprise, Enterprise+ⓘ

The rapid growth of different tools in the modern data stack has helped data professionals address the diverse needs of different teams. The downside of this growth is the fragmentation of business logic across teams, tools, and workloads.<br /><br />

The [Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md) allows you to define metrics in code (with [MetricFlow](../build/about-metricflow.md)) and dynamically generate and query datasets in downstream tools based on their dbt governed assets, such as metrics and models. Integrating with the Semantic Layer will help organizations that use your product make more efficient and trustworthy decisions with their data. It also helps you to avoid duplicative coding, optimize development workflow, ensure data governance, and guarantee consistency for data consumers.

You can use the Semantic Layer for a variety of tools and applications of data. Some common use cases are:

* Business intelligence (BI), reporting, and analytics
* Data quality and monitoring
* Governance and privacy
* Data discovery and cataloging
* Machine learning and data science

[![](/img/icons/dbt-bit.svg)](./sl-graphql.md)

#### [GraphQL API](./sl-graphql.md)

[Use GraphQL to query metrics and dimensions in downstream tools.](./sl-graphql.md)

[![](/img/icons/dbt-bit.svg)](./sl-jdbc.md)

#### [JDBC API](./sl-jdbc.md)

[Use a JDBC driver to query metrics and dimensions in downstream tools, while also providing standard metadata functionality.](./sl-jdbc.md)

[![](/img/icons/dbt-bit.svg)](./sl-python.md)

#### [Python SDK](./sl-python.md)

[Use the Python SDK to interact with the dbt Semantic Layer using Python.](./sl-python.md)
