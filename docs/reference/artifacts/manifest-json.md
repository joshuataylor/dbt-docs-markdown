# Manifest JSON file

| dbt version            | Manifest version                                                                 |
| ---------------------- | -------------------------------------------------------------------------------- |
| dbt Fusion engine v2.0 | v20 (Identical to [v12](https://schemas.getdbt.com/dbt/manifest/v12/index.html)) |
| Core v1.11             | [v12](https://schemas.getdbt.com/dbt/manifest/v12/index.html)                    |
| Core v1.10             | [v12](https://schemas.getdbt.com/dbt/manifest/v12/index.html)                    |
| Core v1.9              | [v12](https://schemas.getdbt.com/dbt/manifest/v12/index.html)                    |
| Core v1.8              | [v12](https://schemas.getdbt.com/dbt/manifest/v12/index.html)                    |
| Core v1.7              | [v11](https://schemas.getdbt.com/dbt/manifest/v11/index.html)                    |
| Core v1.6              | [v10](https://schemas.getdbt.com/dbt/manifest/v10/index.html)                    |
| Core v1.5              | [v9](https://schemas.getdbt.com/dbt/manifest/v9/index.html)                      |
| Core v1.4              | [v8](https://schemas.getdbt.com/dbt/manifest/v8/index.html)                      |
| Core v1.3              | [v7](https://schemas.getdbt.com/dbt/manifest/v7/index.html)                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

**Produced by**: Any [dbt command](https://docs.getdbt.com/category/list-of-commands.md) that parses the project. This includes all commands, *except* [`deps`](https://docs.getdbt.com/reference/commands/deps.md), [`clean`](https://docs.getdbt.com/reference/commands/clean.md), [`debug`](https://docs.getdbt.com/reference/commands/debug.md), and [`init`](https://docs.getdbt.com/reference/commands/init.md).

After executing a dbt command, the `manifest.json` file can be found in the project's `target/` directory:

* If developing locally: Open the `target/` directory in your project folder
* In the Studio IDE: Open the `target/` directory in the file tree
* In dbt platform jobs: Download the `manifest.json` from the [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md) tab for a given job run

This file contains a full representation of your dbt project's resources (models, tests, macros, and more), including all node configurations and resource properties. Even if you're only running some models or tests, all resources will appear in the manifest (unless they are disabled) with most of their properties. Some properties, such as `compiled_sql`, are included only for executed nodes.

Today, dbt uses this file to populate the [docs site](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md), and to perform [state comparison](https://docs.getdbt.com/reference/node-selection/syntax.md#about-node-selection). Members of the community also use it to analyze project health, such as checking for missing descriptions or tests.

### Top-level keys[​](#top-level-keys "Direct link to Top-level keys")

* [`metadata`](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md#common-metadata)
* `nodes`: Dictionary of all analyses, models, seeds, snapshots, and tests.
* `sources`: Dictionary of sources
* `metrics`: Dictionary of metrics
* `exposures`: Dictionary of exposures
* `groups`: Dictionary of groups (**Note:** Added in v1.5)
* `macros`: Dictionary of macros
* `docs`: Dictionary of `docs` blocks
* `parent_map`: Dictionary that contains the first-order parents of each resource
* `child_map`: Dictionary that contains the first-order children of each resource
* `group_map`: Dictionary that maps group names to their resource nodes
* `selectors`: Expanded dictionary representation of [YAML `selectors`](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md)
* `disabled`: Array of resources with `enabled: false`

### Resource details[​](#resource-details "Direct link to Resource details")

All resources nested within `nodes`, `sources`, `metrics`, `exposures`, `macros`, and `docs` have the following base properties:

* `name`: Resource name
* `unique_id`: `<resource_type>.<package>.<resource_name>`, same as dictionary key
* `package_name`: Name of package that defines this resource
* `root_path`: Absolute file path of this resource's package. (**Note:** This was removed for most node types in dbt Core v1.4 / manifest v8 to reduce duplicative information across nodes, but it is still present for seeds.)
* `path`: Relative file path of this resource's definition within its "resource path" (`model-paths`, `seed-paths`, and more).
* `original_file_path`: Relative file path of this resource's definition, including its resource path.

Each has several additional properties related to its resource type.

### dbt JSON schema[​](#dbt-json-schema "Direct link to dbt JSON schema")

You can refer to the [dbt JSON schema](https://schemas.getdbt.com/) for information on describing and consuming dbt-generated artifacts.

**Note**: The `manifest.json` version number is related to (but not *equal* to) your dbt version, so you *must* use the correct `manifest.json` version for your dbt version. To find the correct `manifest.json` version, select the dbt version on the top navigation (such as `v1.5`).

Refer to the table at the beginning of [this page](https://docs.getdbt.com/reference/artifacts/manifest-json.md) to understand how the manifest version matches the dbt version.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
