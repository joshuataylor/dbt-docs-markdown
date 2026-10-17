# dbt\_project.yml

The dbt\_project.yml file is a required file for all dbt projects. It contains important information that tells dbt how to operate your project.

Every [dbt project](../docs/build/projects.md) needs a `dbt_project.yml` file — this is how dbt knows a directory is a dbt project. It also contains important information that tells dbt how to operate your project. It works as follows:

* dbt uses [YAML](https://yaml.org/) in a few different places. If you're new to YAML, it would be worth learning how arrays, dictionaries, and strings are represented.

* By default, dbt looks for the `dbt_project.yml` in your current working directory and its parents, but you can set a different directory using the `--project-dir` flag or the (Applies to dbt v1.11 and later) `DBT_ENGINE_PROJECT_DIR` environment variable.

* Specify your dbt project ID in the `dbt_project.yml` file using `project-id` under the [`dbt-cloud` config](./dbt_cloud.yml.md#the-dbt-cloud-block-in-dbt_projectyml). Find your project ID in your dbt project URL: For example, in `https://YOUR_ACCESS_URL/develop/projects/123456`, the project ID is `123456`.

* Note, you can't set up a "property" in the `dbt_project.yml` file if it's not a config (an example is [macros](./macro-properties.md)). This applies to all types of resources. Refer to [Configs and properties](./configs-and-properties.md) for more detail.

## Example

The following example is a list of all available configurations in the `dbt_project.yml` file:

dbt\_project.yml

```yml
name: string

config-version: 2
version: version

profile: profilename

model-paths: [directorypath]
seed-paths: [directorypath]
test-paths: [directorypath]
analysis-paths: [directorypath]
macro-paths: [directorypath]
snapshot-paths: [directorypath]
docs-paths: [directorypath]
asset-paths: [directorypath]
function-paths: [directorypath]
osi-paths: [directorypath]
check-paths: [directorypath]
skill-paths: [directorypath]

packages-install-path: directorypath

clean-targets: [directorypath]

query-comment: string

require-dbt-version: version-range | [version-range]

flags:
  <global-configs>

dbt-cloud:
  project-id: project_id # Required
  defer-env-id: environment_id # Optional
  account_id: account_id # Optional, v2 only; note the underscore, unlike the other dbt-cloud fields
  account-host: account-host # Defaults to 'cloud.getdbt.com'; Required if use a different Access URL

analyses: # Requires the require_corrected_analysis_fqns flag; available starting v1.12
  <analysis-configs>

exposures:
  +enabled: true | false

quoting:
  database: true | false
  schema: true | false
  identifier: true | false
  snowflake_ignore_case: true | false  # v2-only config. Aligns with Snowflake's session parameter QUOTED_IDENTIFIERS_IGNORE_CASE behavior. 
                                       # Ignored by dbt v1 and other adapters.
metrics:
  <metric-configs>

models:
  <model-configs>

seeds:
  <seed-configs>

semantic-models:
  <semantic-model-configs>

saved-queries:
  <saved-queries-configs>

skills:
  <skill-configs>

snapshots:
  <snapshot-configs>

sources:
  <source-configs>
  
checks:
  <check-configs>

data_tests:
  <test-configs>

info_schema:
  version: 1  # Pins which version of the dbt Information Schema the {{ info_schema() }} macro resolves to

vars:
  <variables>

on-run-start: sql-statement | [sql-statement]
on-run-end: sql-statement | [sql-statement]

dispatch:
  - macro_namespace: packagename
    search_order: [packagename]

restrict-access: true | false

functions:
  <function-configs>
```

Report incorrect code

## The `+` prefix

dbt demarcates between a folder name and a configuration by using a `+` prefix before the configuration name. The `+` prefix is used for configs *only* and applies to `dbt_project.yml` under the corresponding resource key. It doesn't apply to:

* `config()` Jinja macro within a resource file
* config property in a `.yml` file.

For more information, refer to [Using the `+` prefix](./resource-configs/plus-prefix.md).

## Naming convention

It's important to follow the correct YAML naming conventions for the configs in your `dbt_project.yml` file to ensure dbt can process them properly. This is especially true for resource types with more than one word.

* For the multi-word resource types `saved-queries` and `semantic-models`, use dashes (`-`) in `dbt_project.yml`. Here's an example for [saved queries](../docs/build/saved-queries.md#configure-saved-query):

  dbt\_project.yml

  ```yml
  saved-queries:  # Use dashes for saved-queries and semantic-models in dbt_project.yml.
    my_saved_query:
      +cache:
        enabled: true
  ```

  Report incorrect code

* For [data tests](../docs/build/data-tests.md) and [unit tests](../docs/build/unit-tests.md), use underscores (`_`) everywhere, including in `dbt_project.yml`:

  dbt\_project.yml

  ```yml
  data_tests:  # Use underscores for data_tests and unit_tests, even in dbt_project.yml.
    +store_failures: true

  unit_tests:
    +enabled: true
  ```

  Report incorrect code

* For YAML files other than `dbt_project.yml`, use underscores (`_`) for multi-word resource types. For example, the same saved queries resource in a properties file:

  models/saved\_queries.yml

  ```yml
  saved_queries:  # Use underscores outside of dbt_project.yml.
    - name: saved_query_name
      ... # Rest of the saved queries configuration.
      config:
        cache:
          enabled: true
  ```

  Report incorrect code
