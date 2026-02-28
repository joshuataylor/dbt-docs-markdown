# dbt\_project.yml

The dbt\_project.yml file is a required file for all dbt projects. It contains important information that tells dbt how to operate your project.

Every [dbt project](https://docs.getdbt.com/docs/build/projects.md) needs a `dbt_project.yml` file — this is how dbt knows a directory is a dbt project. It also contains important information that tells dbt how to operate your project. It works as follows:

* dbt uses [YAML](https://yaml.org/) in a few different places. If you're new to YAML, it would be worth learning how arrays, dictionaries, and strings are represented.

* By default, dbt looks for the `dbt_project.yml` in your current working directory and its parents, but you can set a different directory using the `--project-dir` flag or the `DBT_PROJECT_DIR` environment variable.

* Specify your dbt project ID in the `dbt_project.yml` file using `project-id` under the `dbt-cloud` config. Find your project ID in your dbt project URL: For example, in `https://YOUR_ACCESS_URL/11/projects/123456`, the project ID is `123456`.

* Note, you can't set up a "property" in the `dbt_project.yml` file if it's not a config (an example is [macros](https://docs.getdbt.com/reference/macro-properties.md)). This applies to all types of resources. Refer to [Configs and properties](https://docs.getdbt.com/reference/configs-and-properties.md) for more detail.

## Example[​](#example "Direct link to Example")

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

packages-install-path: directorypath

clean-targets: [directorypath]

query-comment: string

require-dbt-version: version-range | [version-range]

flags:
  <global-configs>

dbt-cloud:
  project-id: project_id # Required
  defer-env-id: environment_id # Optional
  account-host: account-host # Defaults to 'cloud.getdbt.com'; Required if use a different Access URL

exposures:
  +enabled: true | false

quoting:
  database: true | false
  schema: true | false
  identifier: true | false
  snowflake_ignore_case: true | false  # Fusion-only config. Aligns with Snowflake's session parameter QUOTED_IDENTIFIERS_IGNORE_CASE behavior. 
                                       # Ignored by dbt Core and other adapters.
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

snapshots:
  <snapshot-configs>

sources:
  <source-configs>
  
data_tests:
  <test-configs>

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

## The `+` prefix[​](#the--prefix "Direct link to the--prefix")

<!-- -->

dbt demarcates between a folder name and a configuration by using a `+` prefix before the configuration name. The `+` prefix is used for configs *only* and applies to `dbt_project.yml` under the corresponding resource key. It doesn't apply to:

* `config()` Jinja macro within a resource file
* config property in a `.yml` file.

For more info, see the [Using the `+` prefix](https://docs.getdbt.com/reference/resource-configs/plus-prefix.md).

## Naming convention[​](#naming-convention "Direct link to Naming convention")

It's important to follow the correct YAML naming conventions for the configs in your `dbt_project.yml` file to ensure dbt can process them properly. This is especially true for resource types with more than one word.

* Use dashes (`-`) when configuring resource types with multiple words in your `dbt_project.yml` file. Here's an example for [saved queries](https://docs.getdbt.com/docs/build/saved-queries.md#configure-saved-query):

  dbt\_project.yml

  ```yml
  saved-queries:  # Use dashes for resource types in the dbt_project.yml file.
    my_saved_query:
      +cache:
        enabled: true
  ```

* Use underscore (`_`) when configuring resource types with multiple words for YAML files other than the `dbt_project.yml` file. For example, here's the same saved queries resource in the `semantic_models.yml` file:

  models/semantic\_models.yml

  ```yml
  saved_queries:  # Use underscores everywhere outside the dbt_project.yml file.
    - name: saved_query_name
      ... # Rest of the saved queries configuration.
      config:
        cache:
          enabled: true
  ```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
