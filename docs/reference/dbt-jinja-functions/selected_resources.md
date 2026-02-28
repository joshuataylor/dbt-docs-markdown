# About selected\_resources context variable

The `selected_resources` context variable contains a list of all the *nodes* selected by the current dbt command.

Currently, this variable is not accessible when using the command `run-operation`.

Warning!

dbt actively builds the graph during the [parsing phase](https://docs.getdbt.com/reference/dbt-jinja-functions/execute.md) of running dbt projects, so the `selected_resources` context variable will be empty during parsing. Please read the information on this page to effectively use this variable.

### Usage[​](#usage "Direct link to Usage")

The `selected_resources` context variable is a list of all the resources selected by the current dbt command selector. Its value depends on the usage of parameters like `--select`, `--exclude` and `--selector`.

For a given run it will look like:

```json
["model.my_project.model1", "model.my_project.model2", "snapshot.my_project.my_snapshot"]
```

Each value corresponds to a key in the `nodes` object within the [graph](https://docs.getdbt.com/reference/dbt-jinja-functions/graph.md) context variable.

It can be used in macros in a `pre-hook`, `post-hook`, `on-run-start` or `on-run-end` to evaluate what nodes are selected and trigger different logic whether a particular node is selected or not.

check-node-selected.sql

```sql

/*
  Check if a given model is selected and trigger a different action, depending on the result
*/

{% if execute %}
  {% if 'model.my_project.model1' in selected_resources %}
  
    {% do log("model1 is included based on the current selection", info=true) %}
  
  {% else %}

    {% do log("model1 is not included based on the current selection", info=true) %}

  {% endif %}
{% endif %}

/*
  Example output when running the code in on-run-start 
  when doing `dbt build`, including all nodels
---------------------------------------------------------------
  model1 is included based on the current selection


  Example output when running the code in on-run-start 
  when doing `dbt run --select model2` 
---------------------------------------------------------------
  model1 is not included based on the current selection
*/
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
