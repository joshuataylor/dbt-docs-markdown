# Navigate the dbt Insights interface [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Learn how to navigate Insights interface and use the main components.

Insights provides an interactive interface for writing, running, and analyzing SQL queries. This section highlights the main components of Insights.

## Query console[​](#query-console "Direct link to Query console")

The query console is the main component of Insights. It allows you to write, run, and analyze SQL queries. The Query console supports:

1. Query console editor, which allows you to write, run, and analyze SQL queries:

* It supports syntax highlighting and autocomplete suggestions
* Hyperlink from SQL code `ref` to the corresponding Explorer page

2. [Query console menu](#query-console-menu), which contains **Bookmark (icon)**, **Develop**, and **Run** buttons.
3. [Query output panel](#query-output-panel), below the query editor and displays the results of a query:

* Has three tabs: **Data**, **Chart**, and **Details**, which allow you to analyze query execution and visualize results.

4. [Query console sidebar menu](#query-console-sidebar-menu), which contains the **Catalog**, **Bookmark**, **Query history**, and **dbt Copilot** icons.

[![dbt Insights main interface with blank query editor](/img/docs/dbt-insights/insights-main.png?v=2 "dbt Insights main interface with blank query editor")](#)dbt Insights main interface with blank query editor

### Query console menu[​](#query-console-menu "Direct link to Query console menu")

The Query console menu is located at the top right of the Query editor. It contains the **Bookmark**, **Develop**, and **Run** buttons:

* **Bookmark** button — Save your frequently used SQL queries as favorites for easier access.

  <!-- -->

  * When you click **Bookmark**, a **Bookmark Query Details** modal (pop up box) will appear where you can add a **Title** and **Description**.
  * Let dbt Copilot do the writing for you — use the AI assistant to automatically generate a helpful description for your bookmark.
  * Access the newly created bookmark from the **Bookmark** icon in the [Query console sidebar menu](#query-console-sidebar-menu).

* **Develop**: Open the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) or [Canvas](https://docs.getdbt.com/docs/platform/canvas.md) to continue editing your SQL query.

* **Run** button — Run your SQL query and view the results in the **Data** tab.

## Semantic Layer querying[​](#semantic-layer-querying "Direct link to Semantic Layer querying")

The Semantic Layer querying in dbt Insights lets you build queries against the Semantic Layer without writing SQL code. It guides you in creating queries based on available metrics, dimensions, and entities. With this feature, you can:

* Build analyses from your predefined semantic layer metrics.
* Have filters, time ranges, and aggregates tailored to the semantic model.
* View the underlying SQL code for each metric query.

To build a query in dbt Insights:

1. From the main menu, go to **Insights**.

2. Click **Build a query**.

3. From the Environment dropdown, select **PROD** or **STG**. Even thought you see **DEV** as an option, queries against development environments are deferred based on deferral rules, so selecting it will not query your development environment directly.

4. Select what you want to include in your query.

   * Click **Add Metric** to select the metrics for your query.
   * Click **Add Group by** to choose the dimensions that break down your metric, such as time grain (day, week, month), region, product, or customer.
   * Click **Add Filter** to create a filter to narrow your results.
   * Click **Add Order by** to select how you want to sort the results of your query.
   * Click **Add Limit**, select the amount of results you want to see when you run your query. If left blank, you will get all results.

5. Click **Run** to run your query. Results are available in the **Data** tab. You can see the SQL code generated in the **Details** tab.

   [![Semantic Layer querying within dbt Insights](/img/docs/dbt-insights/insights-query-builder-interface.png?v=2 "Semantic Layer querying within dbt Insights")](#)Semantic Layer querying within dbt Insights

   [![Results are displayed in the Data tab](/img/docs/dbt-insights/insights-query-builder.png?v=2 "Results are displayed in the Data tab")](#)Results are displayed in the Data tab

   [![The generated SQL code in the Details tab](/img/docs/dbt-insights/insights-query-builder-sql.png?v=2 "The generated SQL code in the Details tab")](#)The generated SQL code in the Details tab

## Query output panel[​](#query-output-panel "Direct link to Query output panel")

The Query output panel is below the query editor and displays the results of a query. It displays the following tabs to analyze query execution and visualize results:

* **Data** tab — Preview your SQL results, with results paginated.

* **Details** tab — Generates succinct details of executed SQL query:

  <!-- -->

  * Query metadata — dbt Copilot's AI-generated title and description. Along with the supplied SQL and compiled SQL.
  * Connection details — Relevant data platform connection information.
  * Query details — Query duration, status, column count, row count.

* **Chart** tab — Visualizes query results with built-in charts.

  <!-- -->

  * Use the chart icon to select the type of chart you want to visualize your results. Available chart types are **line chart, bar chart, or scatterplot**.
  * Use the **Chart settings** to customize the chart type and the columns you want to visualize.
  * Available chart types are **line chart, bar chart, or scatterplot**.

* **Download** button — Allows you to export the results to CSV

[![dbt Insights Data tab](/img/docs/dbt-insights/insights-chart-tab.png?v=2 "dbt Insights Data tab")](#)dbt Insights Data tab

[![dbt Insights Chart tab](/img/docs/dbt-insights/insights-chart.png?v=2 "dbt Insights Chart tab")](#)dbt Insights Chart tab

[![dbt Insights Details tab](/img/docs/dbt-insights/insights-details.png?v=2 "dbt Insights Details tab")](#)dbt Insights Details tab

## Query console sidebar menu[​](#query-console-sidebar-menu "Direct link to Query console sidebar menu")

The Query console sidebar menu and icons contains the following options:

### dbt Catalog[​](#dbt-catalog "Direct link to dbt Catalog")

**Catalog icon** — View your project's models, columns, metrics, and more using the integrated Catalog view.

[![dbt Insights dbt Catalog icon](/img/docs/dbt-insights/insights-explorer.png?v=2 "dbt Insights dbt Catalog icon")](#)dbt Insights dbt Catalog icon

### Bookmark[​](#bookmark "Direct link to Bookmark")

Save and access your frequently used queries from the query console sidebar.

To save and manage bookmarks:

1. Click **Bookmark** in the [Query console menu](#query-console-menu) to save the current query. In the **Bookmark Query Details** modal, add a **Title** and **Description**.
2. Select the **Bookmark** icon in the sidebar to open your list of saved queries.
3. Select a saved Insight to view its details or open it in the query editor.

For sharing saved Insights and version history, check out [Save your Insights](https://docs.getdbt.com/docs/explore/access-dbt-insights.md#save-your-insights).

[![Bookmark Query Details modal](/img/docs/dbt-insights/save-insights-modal.png?v=2 "Bookmark Query Details modal")](#)Bookmark Query Details modal

[![Manage your query bookmarks](/img/docs/dbt-insights/manage-bookmarks.png?v=2 "Manage your query bookmarks")](#)Manage your query bookmarks

[![List view of saved Insights](/img/docs/dbt-insights/view-saved-insights.png?v=2 "List view of saved Insights")](#)List view of saved Insights

### Query history[​](#query-history "Direct link to Query history")

View past queries, their statuses (All, Success, Error, or Pending), start time, and duration. Search for past queries and filter by status. You can also re-run a query from the Query history.

[![dbt Insights Query history icon](/img/docs/dbt-insights/insights-query-history.png?v=2 "dbt Insights Query history icon")](#)dbt Insights Query history icon

### dbt Copilot[​](#dbt-copilot "Direct link to dbt Copilot")

<!-- -->

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](https://docs.getdbt.com/docs/platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

Use dbt Copilot to modify or generate queries using natural language prompts or to chat with the Analyst agent to gather insights about your data.

Use dbt Copilot in Insights in two ways:

* **Generate SQL**: Build queries in Insights with natural language prompts to explore and query data with an intuitive, context-rich interface. For more information, see [Build queries in dbt Insights](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md).

* **dbt Copilot Analyst agent** [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles"): Ask questions to get intelligent data analysis with automated workflows, governed insights, and actionable recommendations. This is a conversational AI feature where you can ask natural language prompts and receive analysis in real-time.

  Enable beta features under **Account settings** > **Personal profile** > **Experimental features** to use the Analyst agent. For steps on how to enable, see [Preview new dbt platform features](https://docs.getdbt.com/docs/dbt-versions/experimental-features.md).

  Some sample questions you can ask the agent:

  * *What region are my sales growing the fastest?*
  * *What was the revenue last month?*
  * *How should I optimize my marketing spend next quarter?*
  * *How many customers do I have, broken down by customer type?*

  dbt Copilot Analyst agent creates an analysis plan based on your question. dbt Copilot Analyst agent:

  1. Gets context using your semantic models and metrics.
  2. Generates SQL queries using your project's definitions.
  3. Executes the SQL query and returns results with context.
  4. Reviews and summarizes the generated insights and provides a comprehensive answer.

  The agent can loop through these steps multiple times if it hasn't reached a complete answer, allowing for complex, multi-step analysis.⁠

  For more information, see [dbt Copilot in Insights](https://docs.getdbt.com/docs/dbt-ai/analyst-agent.md).

[![dbt Copilot in Insights](/img/docs/dbt-insights/insights-copilot-tabs.png?v=2 "dbt Copilot in Insights")](#)dbt Copilot in Insights

## LSP features[​](#lsp-features "Direct link to LSP features")

The following Language Server Protocol (LSP) features are available for projects upgraded to Fusion:

* **Live CTE previews:** Preview a CTE’s output for faster validation and debugging.

  [![Preview CTE in Insights](/img/docs/dbt-insights/preview-cte.png?v=2 "Preview CTE in Insights")](#)Preview CTE in Insights

* **Real-time error detection:** Automatically validate your SQL code to detect errors and surface warnings, without hitting the warehouse. This includes both dbt errors (like invalid `ref`) and SQL errors (like invalid column name or SQL syntax).

  [![Live error detection](/img/docs/dbt-insights/sql-validation.png?v=2 "Live error detection")](#)Live error detection

* **`ref` suggestions:** Autocomplete model names when using the `ref()` function to reference other models in your project.

  [![ref suggestions in Insights](/img/docs/dbt-insights/ref-autocomplete.png?v=2 "ref suggestions in Insights")](#)ref suggestions in Insights

* **Hover insights:** View context on tables, columns, and functions without leaving your code. Hover over any SQL element to see details like column names and data types.

  [![Sample column details](/img/docs/dbt-insights/column-info.png?v=2 "Sample column details")](#)Sample column details

  [![Sample column details](/img/docs/dbt-insights/column-hover.png?v=2 "Sample column details")](#)Sample column details
