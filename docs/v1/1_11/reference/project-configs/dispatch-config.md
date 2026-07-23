# dispatch (config)

dbt\_project.yml

```yml
dispatch:
  - macro_namespace: packagename
    search_order: [packagename]
  - macro_namespace: packagename
    search_order: [packagename]
```

## Definition[​](#definition "Direct link to Definition")

Optionally override the [dispatch](https://docs.getdbt.com/reference/dbt-jinja-functions/dispatch.md) search locations for macros in certain namespaces. If not specified, `dispatch` will look in your root project *first*, by default, and then look for implementations in the package named by `macro_namespace`.

## Examples[​](#examples "Direct link to Examples")

I want to "shim" the `dbt_utils` package with the `spark_utils` compatibility package.

dbt\_project.yml

```yml
dispatch:
  - macro_namespace: dbt_utils
    search_order: ['spark_utils', 'dbt_utils']
```

I've reimplemented certain macros from the `dbt_utils` package in my root project (`'my_root_project'`), and I want my versions to take precedence. Otherwise, fall back to the versions in `dbt_utils`.

*Note: This is the default behavior. You may optionally choose to express that search order explicitly as:*

dbt\_project.yml

```yml
dispatch:
  - macro_namespace: dbt_utils
    search_order: ['my_root_project', 'dbt_utils']
```
