# query-comment

dbt\_project.yml

```yml
query-comment: string
```

The `query-comment` configuration also accepts a dictionary input, like so:

dbt\_project.yml

```yml
models:
  my_dbt_project:
    +materialized: table

query-comment:
  comment: string
  append: true | false
  job-label: true | false  # BigQuery only
```

## Definition[​](#definition "Direct link to Definition")

A string to inject as a comment in each query that dbt runs against your database. This comment can attribute SQL statements to specific dbt resources like models and tests.

The `query-comment` configuration can also call a macro that returns a string.

## Default[​](#default "Direct link to Default")

By default, dbt automatically inserts a JSON comment in each query it runs. This comment includes metadata such as the dbt version, profile and target names, and node ID for the resource generating the query.

* For Snowflake, the comment appears at the *end* of the query. This prevents the comment from being stripped during processing.

* For other adapters, the comment appears at the *beginning* of the query. For example:

  ```sql
  /* {"app": "dbt", "dbt_version": "1.10.0rc2", "profile_name": "debug",
      "target_name": "dev", "node_id": "model.dbt2.my_model"} */

  create view analytics.analytics.orders as (
      select ...
    );
  ```

## Using the dictionary syntax[​](#using-the-dictionary-syntax "Direct link to Using the dictionary syntax")

The dictionary syntax includes two keys:

* `comment` (optional, for more information, refer to the [default](#default) section): The string to be injected into a query as a comment.
* `append` (optional, default=`false`): Whether a comment should be appended (added to the bottom of a query) or not (i.e. added to the top of a query). By default, comments are added to the top of queries (i.e. `append: false`).

This syntax is useful on databases like Snowflake which [remove leading SQL comments](https://docs.snowflake.com/en/release-notes/2017-04.html#queries-leading-comments-removed-during-execution).

## Examples[​](#examples "Direct link to Examples")

### Prepend a static comment[​](#prepend-a-static-comment "Direct link to Prepend a static comment")

The following example injects a comment that reads `/* executed by dbt */` into the header of the SQL queries that dbt runs.

dbt\_project.yml

```yml
query-comment: "executed by dbt"
```

**Example output:**

```sql
/* executed by dbt */

select ...
```

### Disable query comments[​](#disable-query-comments "Direct link to Disable query comments")

dbt\_project.yml

```yml
query-comment:
```

Or:

dbt\_project.yml

```yml
query-comment: null
```

### Prepend a dynamic comment[​](#prepend-a-dynamic-comment "Direct link to Prepend a dynamic comment")

The following example injects a comment that varies based on the configured `user` specified in the active dbt target.

dbt\_project.yml

```yml
query-comment: "run by {{ target.user }} in dbt"
```

**Example output:**

```sql
/* run by drew in dbt */

select ...
```

### Append the default comment[​](#append-the-default-comment "Direct link to Append the default comment")

The following example uses the dictionary syntax to append (rather than prepend) the default comment.

Note that the `comment:` field is omitted to allow the default to be appended.

dbt\_project.yml

```yaml

query-comment:
  append: True
```

**Example output:**

```sql
select ...
/* {"app": "dbt", "dbt_version": "1.6.0rc2", "profile_name": "debug", "target_name": "dev", "node_id": "model.dbt2.my_model"} */
;
```

### BigQuery: include query comment items as job labels[​](#bigquery-include-query-comment-items-as-job-labels "Direct link to BigQuery: include query comment items as job labels")

If `query-comment.job-label` is set to true, dbt will include the query comment items, if a dictionary, or the comment string, as job labels on the query it executes. These will be included in addition to labels specified in the [BigQuery-specific config](https://docs.getdbt.com/reference/project-configs/query-comment.md#bigquery-include-query-comment-items-as-job-labels).

dbt\_project.yml

```yaml

query-comment:
  job-label: True
```

### Append a custom comment[​](#append-a-custom-comment "Direct link to Append a custom comment")

The following example uses the dictionary syntax to append (rather than prepend) a comment that varies based on the configured `user` specified in the active dbt target.

dbt\_project.yml

```yaml

query-comment:
  comment: "run by {{ target.user }} in dbt"
  append: True
```

**Example output:**

```sql
select ...
/* run by drew in dbt */
;
```

### Intermediate: Use a macro to generate a comment[​](#intermediate-use-a-macro-to-generate-a-comment "Direct link to Intermediate: Use a macro to generate a comment")

The `query-comment` config can reference macros in your dbt project. Simply create a macro with any name (`query_comment` is a good start!) in your `macros` directory, like so:

macros/query\_comment.sql

```jinja2

{% macro query_comment() %}

  dbt {{ dbt_version }}: running {{ node.unique_id }} for target {{ target.name }}

{% endmacro %}
```

Then call the macro in your `dbt_project.yml` file. Make sure you quote the macro to avoid the YAML parser from trying to interpret the `{` as the start of a dictionary.

dbt\_project.yml

```yaml
query-comment: "{{ query_comment() }}"
```

### Advanced: Use a macro to generate a comment[​](#advanced-use-a-macro-to-generate-a-comment "Direct link to Advanced: Use a macro to generate a comment")

The following example shows a JSON query comment which can be parsed to understand the performance characteristics of your dbt project.

macros/query\_comment.sql

```jinja2
{% macro query_comment(node) %}
    {%- set comment_dict = {} -%}
    {%- do comment_dict.update(
        app='dbt',
        dbt_version=dbt_version,
        profile_name=target.get('profile_name'),
        target_name=target.get('target_name'),
    ) -%}
    {%- if node is not none -%}
      {%- do comment_dict.update(
        file=node.original_file_path,
        node_id=node.unique_id,
        node_name=node.name,
        resource_type=node.resource_type,
        package_name=node.package_name,
        relation={
            "database": node.database,
            "schema": node.schema,
            "identifier": node.identifier
        }
      ) -%}
    {% else %}
      {%- do comment_dict.update(node_id='internal') -%}
    {%- endif -%}
    {% do return(tojson(comment_dict)) %}
{% endmacro %}
```

As above, call this macro as follows:

dbt\_project.yml

```yaml
query-comment: "{{ query_comment(node) }}"
```

## Compilation context[​](#compilation-context "Direct link to Compilation context")

The following context variables are available when generating a query comment:

| Context Variable | Description                                                                                                                                                   |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dbt\_version     | The version of dbt being used. For details about release versioning, refer to [Versioning](https://docs.getdbt.com/reference/commands/version.md#versioning). |
| env\_var         | See [env\_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md)                                                                              |
| modules          | See [modules](https://docs.getdbt.com/reference/dbt-jinja-functions/modules.md)                                                                               |
| run\_started\_at | When the dbt invocation began                                                                                                                                 |
| invocation\_id   | A unique ID for the dbt invocation                                                                                                                            |
| fromjson         | See [fromjson](https://docs.getdbt.com/reference/dbt-jinja-functions/fromjson.md)                                                                             |
| tojson           | See [tojson](https://docs.getdbt.com/reference/dbt-jinja-functions/tojson.md)                                                                                 |
| log              | See [log](https://docs.getdbt.com/reference/dbt-jinja-functions/log.md)                                                                                       |
| var              | See [var](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md)                                                                                       |
| target           | See [target](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md)                                                                                 |
| connection\_name | A string representing the internal name for the connection. This string is generated by dbt.                                                                  |
| node             | A dictionary representation of the parsed node object. Use `node.unique_id`, `node.database`, `node.schema`, and so on.                                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Note: The `var()` function in `query-comment` macros only access variables passed through the `--vars` argument in the CLI. Variables defined in the vars block of your `dbt_project.yml` are not accessible when generating query comments.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
