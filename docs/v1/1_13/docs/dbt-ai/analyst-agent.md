# dbt Copilot in Insights [No longer sold](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt platform | Enterprise, Enterprise+

dbt Copilot is no longer sold

Starting November 19, 2026, Copilot is no longer being sold or enabled for new dbt platform accounts and this documentation is archived. Existing customers can continue to use Copilot. For a powerful and robust AI agent purpose-built for dbt, check out [dbt Wizard](../platform/wizard-overview.md).

dbt Copilot in Insights lets you chat with your data and get accurate answers powered by the [dbt Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md). Unlike generic AI chat interfaces, dbt Wizard in Insights provides consistent, explainable results with transparent SQL, lineage, and data policies.

## Prerequisites

* Enable beta features under **Account settings** > **Personal profile** > **Experimental features**. See [Preview new dbt platform features](../dbt-versions/experimental-features.md) for steps.
* Have access to [dbt Insights](../explore/dbt-insights.md) and meet those prerequisites.
* Be on a dbt platform [Enterprise-tier](https://www.getdbt.com/pricing) plan — [book a demo](https://www.getdbt.com/contact) to learn more about Insights.
* Available on all [tenant](../platform/about-platform/tenancy.md) configurations.
* Have a dbt [developer license](../platform/manage-access/seats-and-users.md) with access to Insights.
* Configured [user credentials](../platform/studio-ide/develop-in-studio.md#get-started-with-the-studio-ide).

## Using dbt Copilot in Insights

Use dbt Copilot to analyze your data and get contextualized results in real time by asking natural language questions to the [Insights](../explore/dbt-insights.md) dbt Wizard in Insights agent.

1. Click the **dbt Copilot** icon in the Query console sidebar menu.

2. In the dropdown menu above the dbt Copilot prompt box, select **Agent**.

3. In the dbt Copilot prompt box, enter your question.

4. Click **↑** to submit your question.

   The agent then translates natural language questions into structured queries, executes queries against governed dbt models and metrics, and returns results with references, assumptions, and possible next steps.

   The agent can loop through these steps multiple times if it hasn't reached a complete answer, allowing for complex, multi-step analysis.⁠

   dbt Insights automatically executes the SQL query suggested by dbt Copilot in Insights, and you can preview the SQL results in the **Data** tab.

5. Confirm the results or continue asking the agent for more insights about your data.

Your conversation with the agent remains even if you switch tabs within dbt Insights. However, they disappear when you navigate out of Insights or when you close your browser.

![Using dbt Copilot in Insights](/img/docs/dbt-insights/insights-copilot-agent.png?v=2 "Using dbt Copilot in Insights")Using dbt Copilot in Insights
