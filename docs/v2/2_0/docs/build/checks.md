# Checks

Available in v2

As dbt projects grow and more contributors add models, maintaining consistent standards becomes harder: a model ships without a description, a public model has no owner, a new model ignores naming conventions. None of this breaks anything, so it's not caught, but quality erodes silently.

Checks are SQL queries that assert rules and standards about your project metadata. Define rules (such as every model must have a description, required tags are set, and so on) and dbt enforces them before any warehouse work runs.

Write your checks in DuckDB SQL, using the [`{{ info_schema() }}` macro](../../reference/dbt-jinja-functions/info-schema-macro.md) to reference the [dbt Information Schema](./dbt-information-schema.md) in your query. Checks run locally in DuckDB, so they don't require a connection to your warehouse. If the project violates a rule, `dbt build` stops before compiling or materializing a single model.

Similar to [data tests](./data-tests.md), a check finds any instances in your project that do not meet your expectations. A check passes when the query returns zero rows, and fails when it returns one or more.

Checks run automatically with every `dbt build`; you can use `--skip-checks` to skip them. You can also run them on demand with `dbt check`.

## Guidelines for writing SQL check files

This section covers the rules and constraints for writing check SQL files and configuring check behavior.

* A check is a SQL file in your `checks/` directory. You can optionally pair it with a properties YAML file in the same directory to configure it. To use a different directory, set [`check-paths`](../../reference/project-configs/check-paths.md) in `dbt_project.yml`.
* The filename without the `.sql` extension becomes the check name (for example, `all_models_have_descriptions` is the check name for `checks/all_models_have_descriptions.sql`).
* Jinja in check files renders at parse time. You can use Jinja, but the result must be valid DuckDB SQL at that point; checks do not go through a separate compile step the way models do.
* Checks are dbt resources; each check appears in `manifest.json`, supports `tags` and `meta`, and `dbt ls` lists them.

### The `info_schema()` macro

[`{{ info_schema() }}`](../../reference/dbt-jinja-functions/info-schema-macro.md) is the supported way to reference the [dbt Information Schema](./dbt-information-schema.md) in a check. Pass the name of the table you want to query (for example, `{{ info_schema('models') }}` for models or `{{ info_schema('edges') }}` for DAG edges). No materialized [Information Schema](../../reference/info-schema.md) files are required; checks run against an intermediate representation built at parse time.

For the full list of available views and columns, refer to [Columns available for checks](../../reference/info-schema.md#columns-available-for-checks).

## Writing your first check

The following steps walk you through creating your first check.

1. Declare the `info_schema` version in `dbt_project.yml`:

   The `info_schema.version` pins which version of the info schema the [`{{ info_schema() }}`](../../reference/dbt-jinja-functions/info-schema-macro.md) macro resolves to. Currently, `1` is the only available version.

   dbt\_project.yml

   ```yaml
   info_schema:
     version: 1
   ```

2. Write a check under `checks/`:

   Use [`{{ info_schema() }}`](../../reference/dbt-jinja-functions/info-schema-macro.md) to query project metadata. A check passes if the query returns zero rows. For available views and columns, refer to [Columns available for checks](../../reference/info-schema.md#columns-available-for-checks).

   checks/all\_models\_have\_descriptions.sql

   ```sql
   select unique_id
   from {{ info_schema('models') }}
   where description = ''
   ```

3. Configure the check in a properties YAML file in your `checks/` directory.

   checks/\_checks.yml

   ```yaml
   version: 2
   checks:
     - name: all_models_have_descriptions
       description: "Fails if any model is missing a description."
       config:
         severity: warn   # default is error; 'warn' logs issues but does not fail the execution
   ```

4. Run your checks:

   ```shell
   dbt check
   ```

## Example checks

The following examples show common project quality rules, each defined as a SQL query against the `info_schema` macros and saved as a `.sql` file under `checks/`.

* Enforce that all `public` models have a description:

  checks/public\_models\_have\_descriptions.sql

  ```sql
  select unique_id
  from {{ info_schema('models') }}
  where access = 'public'
    and description = ''
  ```

* Flag sources that aren't referenced by models. An unreferenced source has no downstream models, and is either unused or missing a model that should reference it:

  checks/unused\_sources.sql

  ```sql
  select s.unique_id, s.name
  from {{ info_schema('sources') }} as s
  left join {{ info_schema('edges') }} as e on e.parent_unique_id = s.unique_id
  where e.child_unique_id is null
  ```

* Flag `public` models with no group owner:

  checks/public\_models\_have\_owners.sql

  ```sql
  select m.unique_id
  from {{ info_schema('models') }} as m
  left join {{ info_schema('groups') }} as g on g.name = m."group"
  where m.access = 'public'
    and (
      g.unique_id is null
      or (
        (g.owner_name is null or trim(g.owner_name) = '')
        and (g.owner_email is null or trim(g.owner_email) = '')
      )
    )
  ```

## Commands

Checks run with `dbt check` and `dbt build`.

| Command                       | Behavior                                                                                                                                                                                                  |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dbt check`                   | Runs all checks.                                                                                                                                                                                          |
| `dbt check <name1> <name2> …` | Runs only the named checks. An unknown check name is an error; a disabled check name is accepted and skipped.                                                                                             |
| `dbt build`                   | Runs all enabled checks before models compile. A failing check stops the run before any model is compiled or executed. Warn failures are reported and the build continues. Use `--skip-checks` to bypass. |

## Skipping checks on build

To skip all checks, pass the [`--skip-checks` flag](../../reference/commands/build.md?version=2#the---skip-checks-flag) to `dbt build`.

```shell
dbt build --skip-checks
```

To skip a specific check, set `enabled: false` in its config block in the YAML file. The check still appears in the manifest but does not run.

```yaml
checks:
  - name: all_models_have_descriptions
    config:
      enabled: false
```

Disabling at the project level

Setting `+enabled: false` in `dbt_project.yml` disables the check silently — nothing in the output shows checks were skipped. A successful `dbt build` in CI doesn't tell you whether the project has no checks or all checks are disabled. Running `dbt check <name>` for a disabled check also succeeds without running the check or returning an error.

## Using selectors with checks

With checks, [`--select` and other selector methods](../../reference/node-selection/syntax.md) choose which project resources to evaluate, not which checks run.

Why checks work this way:

* You generally don't need to exclude checks or run only a subset of them. Checks are fast. Error-severity checks should block execution if violated; if a rule is informational, set it to `warn`. If a check is no longer relevant, disable or delete it.

* You may want to limit which resources are checked. This lets you incrementally introduce checks in an existing project. In development, run `dbt build --select <the part of your DAG you're working on>` to check only those resources. In CI, your checks run only against modified resources.

* When developing a new check, you can run one check at a time or optionally select resources:

  * `dbt check name_of_check`
  * `dbt check name_of_check --select <resources to check>`

You can also preview any `info_schema` query directly: `dbt show --inline "select * from {{ info_schema('...') }}"`. For example, to inspect your checks' own metadata, run `dbt show --inline "select * from {{ info_schema('checks') }}"`.

### How `selection_filter_on` works

By default, dbt filters check results to selected resources by matching the `unique_id` column in the output. If the check returns no `unique_id` column, it runs against the whole project.

Use [`selection_filter_on`](../../reference/resource-configs/selection-filter-on.md) when your check returns rows with different ID columns that you want to filter on. It accepts the following values:

* **Default (not set):** If the query returns `unique_id`, dbt keeps only rows whose `unique_id` is in the selection. If the query does not return `unique_id`, the check runs against the whole project. Useful for aggregate checks like "the project has at least one model".
* **`none`:** The check always runs against the whole project, ignoring any selector. Use this to make whole-project behavior explicit.
* **A column name or list of column names:** dbt keeps a row if the ID in any of the named columns is in the selection. Each named column must exist in the result, or the check raises an error. Use this for checks that return relationships between resources (edges).

For example, the following check returns `child_unique_id` instead of `unique_id`, so you must set `selection_filter_on`:

checks/multiple\_sources\_joined.sql

```sql
select child_unique_id, count(*) as source_parents
from {{ info_schema('edges') }}
where parent_unique_id like 'source.%'
group by child_unique_id
having count(*) > 1
```

checks/\_checks.yml

```yaml
checks:
  - name: multiple_sources_joined
    description: "Fails if any model reads directly from more than one source."
    config:
      selection_filter_on: child_unique_id
```

## Related documentation

* [Check properties](../../reference/check-properties.md)
* [Check configurations](../../reference/check-configs.md)
* [`dbt check` command](../../reference/commands/check.md)
* [`info_schema`](../../reference/dbt-jinja-functions/info-schema-macro.md)
* [`check-paths` project config](../../reference/project-configs/check-paths.md)
