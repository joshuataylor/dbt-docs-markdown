# About dbt Copilot [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Copilot is a powerful, AI-powered assistant fully integrated into your dbt experience—designed to accelerate your analytics workflows.

Copilot embeds AI-driven assistance across every stage of the [analytics development life cycle (ADLC)](https://www.getdbt.com/resources/guides/the-analytics-development-lifecycle) and harnesses rich metadata—capturing relationships, lineage, and context — so you can deliver refined, trusted data products at speed.

With automatic code generation and using natural language prompts, Copilot can [generate code](https://docs.getdbt.com/docs/cloud/use-dbt-copilot.md), [documentation](https://docs.getdbt.com/docs/build/documentation.md), [data tests](https://docs.getdbt.com/docs/build/data-tests.md), [metrics](https://docs.getdbt.com/docs/build/metrics-overview.md), and [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md) for you with the click of a button in the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-copilot.md), [Canvas](https://docs.getdbt.com/docs/cloud/build-canvas-copilot.md), and [Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md). To learn about prompt best practices, check out the [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md).

tip

Copilot is available on Starter, Enterprise, and Enterprise+ accounts. [Book a demo](https://www.getdbt.com/contact) to see how AI-driven development can streamline your workflow.

[![Example of using dbt Copilot to generate documentation in the IDE](/img/docs/dbt-cloud/cloud-ide/dbt-copilot-doc.gif?v=2 "Example of using dbt Copilot to generate documentation in the IDE")](#)Example of using dbt Copilot to generate documentation in the IDE

## How dbt Copilot works[​](#how-dbt-copilot-works "Direct link to How dbt Copilot works")

Copilot enhances efficiency by automating repetitive tasks while ensuring data privacy and security. It works as follows:

* Access Copilot through:

  <!-- -->

  * The [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-copilot.md) to generate documentation, tests, semantic models.
  * The [Canvas ](https://docs.getdbt.com/docs/cloud/build-canvas-copilot.md)to generate SQL code using natural language prompts. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")
  * The [Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md) to generate SQL queries for analysis using natural language prompts. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

* Copilot gathers metadata (like column names, model SQL, documentation) but never accesses row-level warehouse data.

* The metadata and user prompts are sent to the AI provider (in this case, OpenAI) through API calls for processing.

* The AI-generated content is returned to dbt for you to review, edit, and save within your project files.

* Copilot does not use warehouse data to train AI models.

* No sensitive data persists on dbt Labs' systems, except for usage data.

* Client data, including any personal or sensitive data inserted into the query by the user, is deleted within 30 days by OpenAI.

* Copilot uses a best practice style guide to ensure consistency across teams.

tip

Copilot accelerates, but doesn’t replace, your analytics engineer. It helps deliver better data products faster, but always review AI-generated content, as it may be incorrect. To learn about prompt best practices, check out the [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
