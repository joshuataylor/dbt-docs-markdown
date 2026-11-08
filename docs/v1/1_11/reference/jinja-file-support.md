# Jinja support by file type

Jinja support varies by file type. Some files support Jinja expressions, built-in dbt functions, and custom macros. Others support only a few built-in functions, and some don't render Jinja at all. Use this page to check what each file type supports.

This page covers three kinds of support:

* **Jinja expressions** (`{{ }}`, `{% %}`, and `{# #}`): Control flow, string output, and comments
* **Built-in dbt functions and variables**: Context methods like [`env_var()`](./dbt-jinja-functions/env_var.md), [`ref()`](./dbt-jinja-functions/ref.md), and [`target`](./dbt-jinja-functions/target.md)
* **Custom macros**: Project-defined macros like `{{ my_macro() }}`

Jinja expressions are how you write dynamic logic in a file. Custom macros are one kind of custom Jinja block you define with tags like `{% macro %}`. Other block types include `{% test %}`, `{% materialization %}`, `{% snapshot %}`, and `{% docs %}`. Refer to [Where to define custom Jinja blocks](#where-to-define-custom-jinja-blocks) for where each belongs.

## Summary table

| File type                                         | Jinja expressions      | dbt functions and variables | Custom macros          | Notes                                                                                                                                                                                                                                          |
| ------------------------------------------------- | ---------------------- | --------------------------- | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.sql` models, analyses, singular tests           | ✅                     | ✅                          | ✅                     | Call macros here, including hooks and operations. Refer to [Where to define custom Jinja blocks](#where-to-define-custom-jinja-blocks).                                                                                                        |
| `.sql` generic tests (`{% test %}`)               | ✅                     | ✅                          | ✅                     |                                                                                                                                                                                                                                                |
| `.sql` snapshots (legacy `{% snapshot %}`)        | ✅                     | ✅                          | ✅                     | Prefer [YAML snapshots](../docs/build/snapshots.md).                                                                                                                                                                      |
| `.sql` materializations (`{% materialization %}`) | ✅                     | ✅                          | ✅                     |                                                                                                                                                                                                                                                |
| `.md` docs blocks (`{% docs %}`)                  | ✅                     | ❌                          | ❌                     | Pure Jinja only; use [`doc()`](./dbt-jinja-functions/doc.md) to reference blocks.                                                                                                                              |
| `dbt_project.yml`                                 | ✅                     | Limited                     | v1: ❌ / v2: Limited   | Custom macros in v2 are Limited to hooks. The top-level `vars:` block is literal (not Jinja). Refer to [dbt\_project.yml context](./dbt-jinja-functions/dbt-project-yml-context.md).                           |
| `profiles.yml`                                    | ✅                     | Limited                     | ❌                     | Supports the `DBT_ENV_SECRET_` prefix for [`env_var()`](./dbt-jinja-functions/env_var.md). Refer to [profiles.yml context](./dbt-jinja-functions/profiles-yml-context.md).     |
| `packages.yml`                                    | ✅                     | Limited                     | ❌                     | Supports the `DBT_ENV_SECRET_` prefix for [`env_var()`](./dbt-jinja-functions/env_var.md). Refer to [packages.yml context](<https://docs.getdbt.com/reference/dbt-jinja-functions/packages.yml context.md>).   |
| `properties.yml` / `schema.yml`                   | ✅                     | Limited                     | v1: ❌ / v2: ✅        | In v2 you can call custom macros in YAML values (for example `description`). `ref()` and `source()` are not available. Refer to [properties.yml context](./dbt-jinja-functions/dbt-properties-yml-context.md). |
| `selectors.yml`                                   | ✅                     | Limited                     | v1: ❌ / v2: ✅        | Also supports `env_var()`, `target`, `as_bool()`, and related context. Refer to [YAML selectors](./node-selection/yaml-selectors.md).                                                                          |
| `dependencies.yml`                                | v1: ❌ / v2: ✅        | v1: ❌ / v2: Limited        | ❌                     | In v2, Jinja works (for example `env_var()` in a git URL). Prefer `packages.yml` if the project must also run on v1.                                                                                                                           |
| `.py` Python models                               | ❌                     | ❌                          | ❌                     | Use the Python `dbt` class API instead.                                                                                                                                                                                                        |
| `.csv` seeds                                      | ❌                     | ❌                          | ❌                     | Data files only.                                                                                                                                                                                                                               |
| Unit test SQL fixtures                            | ❌                     | ❌                          | ❌                     | Refer to [data formats](./resource-properties/data-formats.md).                                                                                                                                                |
| `.sql.j2` / `.md.j2` (optional)                   | Same as `.sql` / `.md` | Same as `.sql` / `.md`      | Same as `.sql` / `.md` | Requires [`allow_jinja_file_extensions`](./global-configs/behavior-flags/allow_jinja_file_extensions.md) (`false` by default).                                                                                 |

**Table key**

* **Limited:** Only a subset of built-in dbt functions and variables is available (typically [`env_var()`](./dbt-jinja-functions/env_var.md), [`var()`](./dbt-jinja-functions/var.md), and [`target`](./dbt-jinja-functions/target.md)). The exact list depends on the file type.
* For custom macros, Limited means support only in specific fields (for example hooks in `dbt_project.yml` in v2).
* **Same as `.sql` / `.md`:** Optional `.j2` extensions match the non-`.j2` file type (for example `.sql.j2` matches `.sql`).

## Where to define custom Jinja blocks

Calling a macro (`{{ my_macro() }}`) uses Jinja expressions. Defining one uses a custom Jinja block (`{% macro %}`). Use the table below for where each block type belongs:

| Block                   | File type | Location                      | Notes                                                                                                                                                                                                                   |
| ----------------------- | --------- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `{% macro %}`           | `.sql`    | `macros/`                     | Call from models, tests, hooks, and more.                                                                                                                                                                               |
| `{% test %}`            | `.sql`    | `tests/generic/` or `macros/` | Can call other macros.                                                                                                                                                                                                  |
| `{% materialization %}` | `.sql`    | `macros/`                     | Define custom materializations here.                                                                                                                                                                                    |
| `{% snapshot %}`        | `.sql`    | `snapshots/` (legacy)         | Prefer [YAML snapshots](../docs/build/snapshots.md). Refer to [Legacy snapshot configurations](./resource-configs/snapshots-jinja-legacy.md) for the older syntax. |
| `{% docs %}`            | `.md`     | Any resource path or `docs/`  | Pure Jinja only; no custom macro calls.                                                                                                                                                                                 |

## Common exceptions

Some config fields are stricter than the summary table.

### `vars:` block in `dbt_project.yml`

Values in the `vars:` block in `dbt_project.yml` are literal, not Jinja. To set values dynamically, use `--vars` on the command line. Refer to [Project variables](../docs/build/project-variables.md).

### `sql_header` config

The [`sql_header`](./resource-configs/sql_header.md) config doesn't support Jinja or macros like `ref()`, `source()`, and `{{ this }}`.

## Jinja differences between v1 and v2

v2 supports some Jinja that v1 doesn't:

* Custom macros in `properties.yml` and `selectors.yml` values
* Custom macros in `dbt_project.yml` hooks (Limited)
* Jinja in `dependencies.yml`

If your project must also run on v1, don't use these. In v1, a custom macro in a YAML file fails with an undefined macro error. Jinja in `dependencies.yml` stays an unrendered string that dbt passes to git, which typically fails the install.

This example calls a custom macro in a property value. It works in v2 but fails in v1:

models/schema.yml

```yaml
models:
  - name: customers
    description: "{{ my_macro() }}"
```

This example uses `env_var()` in a package spec. Put it in `packages.yml` so it works in v1 and v2:

packages.yml

```yaml
packages:
  - git: "https://{{ env_var('DBT_ENV_SECRET_GIT_CREDENTIAL') }}@github.com/dbt-labs/awesome_repo.git"
```

For more on package files, refer to [Packages use cases](../docs/build/packages.md#use-cases).

## Related docs

* [Jinja and macros](../docs/build/jinja-macros.md)
* [YAML tips](../docs/build/dbt-tips.md#yaml-tips)
* [dbt Jinja functions and context variables](./dbt-jinja-functions-context-variables.md)
* [Packages use cases](../docs/build/packages.md#use-cases)
