# Does the Cost Insights feature incur warehouse costs?


dbt issues lightweight, read-only queries against your warehouse to retrieve metadata and to power features such as Cost Insights. dbt scopes and filters these queries to minimize impact, and most customers see negligible costs (typically on the order of cents).