# dbt Copilot in Insights [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt platform | Enterprise, Enterprise+ⓘ

dbt Copilot in Insights lets you chat with your data and get accurate answers powered by the [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md). Unlike generic AI chat interfaces, dbt Wizard in Insights provides consistent, explainable results with transparent SQL, lineage, and data policies.

<!-- -->

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](https://docs.getdbt.com/docs/platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Enable beta features under **Account settings** > **Personal profile** > **Experimental features**. See [Preview new dbt platform features](https://docs.getdbt.com/docs/dbt-versions/experimental-features.md) for steps.
* Have access to [dbt Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md) and meet those prerequisites.
* Be on a dbt platform [Enterprise-tier](https://www.getdbt.com/pricing) plan — [book a demo](https://www.getdbt.com/contact) to learn more about Insights.
* Available on all [tenant](https://docs.getdbt.com/docs/platform/about-platform/tenancy.md) configurations.
* Have a dbt [developer license](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md) with access to Insights.
* Configured [user credentials](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#get-started-with-the-studio-ide).

## Using dbt Copilot in Insights[​](#using-dbt-copilot-in-insights "Direct link to Using dbt Copilot in Insights")

<!-- -->

Use dbt Copilot to analyze your data and get contextualized results in real time by asking natural language questions to the [Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md) dbt Wizard in Insights agent.

1. Click the **dbt Copilot** icon in the Query console sidebar menu.

2. In the dropdown menu above the dbt Copilot prompt box, select **Agent**.

3. In the dbt Copilot prompt box, enter your question.

4. Click **↑** to submit your question.

   The agent then translates natural language questions into structured queries, executes queries against governed dbt models and metrics, and returns results with references, assumptions, and possible next steps.

   The agent can loop through these steps multiple times if it hasn't reached a complete answer, allowing for complex, multi-step analysis.⁠

   dbt Insights automatically executes the SQL query suggested by dbt Copilot in Insights, and you can preview the SQL results in the **Data** tab.

5. Confirm the results or continue asking the agent for more insights about your data.

Your conversation with the agent remains even if you switch tabs within dbt Insights. However, they disappear when you navigate out of Insights or when you close your browser.

[![Using dbt Copilot in Insights](/img/docs/dbt-insights/insights-copilot-agent.png?v=2 "Using dbt Copilot in Insights")](#)Using dbt Copilot in Insights
