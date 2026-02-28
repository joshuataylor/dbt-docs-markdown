# Analyst agent [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

The Analyst agent lets you chat with your data and get accurate answers powered by the [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md). Unlike generic AI chat interfaces, the Analyst agent provides consistent, explainable results with transparent SQL, lineage, and data policies.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Enable beta features under **Account settings** > **Personal profile** > **Experimental features**. See [Preview new dbt platform features](https://docs.getdbt.com/docs/dbt-versions/experimental-features.md) for steps.
* Have access to [dbt Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md) and meet those prerequisites.
* Be on a dbt platform [Enterprise-tier](https://www.getdbt.com/pricing) plan — [book a demo](https://www.getdbt.com/contact) to learn more about Insights.
* Available on all [tenant](https://docs.getdbt.com/docs/cloud/about-cloud/tenancy.md) configurations.
* Have a dbt [developer license](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md) with access to Insights.
* Configured [developer credentials](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md#get-started-with-the-cloud-ide).

## Using the Analyst agent[​](#using-the-analyst-agent "Direct link to Using the Analyst agent")

<!-- -->

Use dbt Copilot to analyze your data and get contextualized results in real time by asking natural language questions to the [Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md) Analyst agent.

1. Click the **Copilot** icon in the Query console sidebar menu.

2. In the dropdown menu above the Copilot prompt box, select **Agent**.

3. In the dbt Copilot prompt box, enter your question.

4. Click **↑** to submit your question.

   The agent then translates natural language questions into structured queries, executes queries against governed dbt models and metrics, and returns results with references, assumptions, and possible next steps.

   The agent can loop through these steps multiple times if it hasn't reached a complete answer, allowing for complex, multi-step analysis.⁠

   dbt Insights automatically executes the SQL query suggested by the Analyst agent, and you can preview the SQL results in the **Data** tab.

5. Confirm the results or continue asking the agent for more insights about your data.

Your conversation with the agent remains even if you switch tabs within dbt Insights. However, they disappear when you navigate out of Insights or when you close your browser.

[![Using the Analyst agent in Insights](/img/docs/dbt-insights/insights-copilot-agent.png?v=2 "Using the Analyst agent in Insights")](#)Using the Analyst agent in Insights

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
