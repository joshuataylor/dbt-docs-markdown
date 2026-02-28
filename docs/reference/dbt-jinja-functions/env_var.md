# About env\_var function

The `env_var` function can be used to incorporate environment variables from the system into your dbt project. You can use the `env_var` function in your `profiles.yml` file, the `dbt_project.yml` file, the `sources.yml` file, your `schema.yml` files, and in model `.sql` files. Essentially, `env_var` is available anywhere dbt processes Jinja code.

When used in a `profiles.yml` file (to avoid putting credentials on a server), it can be used like this:

profiles.yml

```yaml
profile:
  target: prod
  outputs:
    prod:
      type: postgres
      host: 127.0.0.1
      # IMPORTANT: Make sure to quote the entire Jinja string here
      user: "{{ env_var('DBT_USER') }}"
      password: "{{ env_var('DBT_PASSWORD') }}"
      ....
```

If the `DBT_USER` and `DBT_ENV_SECRET_PASSWORD` environment variables are present when dbt is invoked, then these variables will be pulled into the profile as expected. If any environment variables are not set, then dbt will raise a compilation error.

### Converting env\_vars[​](#converting-env_vars "Direct link to Converting env_vars")

Environment variables are always strings. When using them for configurations that expect integers or booleans, you must explicitly convert the value to the correct type.

Use a Jinja filter to convert the string to the correct type:

* **Integers** — Convert the string to a number using the `int` or [`as_number`](https://docs.getdbt.com/reference/dbt-jinja-functions/as_number.md) filter to avoid errors like `'1' is not of type 'integer'`. For example, `"{{ env_var('DBT_THREADS') | int }}"` or `"{{ env_var('DB_PORT') | as_number }}"`.

* **Booleans** — Convert the string to a boolean explicitly using the [`as_bool`](https://docs.getdbt.com/reference/dbt-jinja-functions/as_bool.md) filter. For example, `"{{ env_var('DBT_PERSIST_DOCS_RELATION', False) | as_bool }}"`.

For boolean defaults, use capitalized `True` or `False`. Using lowercase `true` or `false` will be treated as a string and can result in unexpected results.

For example, to disable [`persist_docs`](https://docs.getdbt.com/reference/resource-configs/persist_docs.md) using environment variables:

dbt\_project.yml

```yml
+persist_docs:
  relation: "{{ env_var('DBT_PERSIST_DOCS_RELATION', False) | as_bool }}"
  columns: "{{ env_var('DBT_PERSIST_DOCS_COLUMNS', False) | as_bool }}"
```

Quoting, curly brackets, & you

Be sure to quote the entire Jinja string. Otherwise, the YAML parser will be confused by the Jinja curly brackets.

### Default values[​](#default-values "Direct link to Default values")

You can also provide a default value as a second argument:

dbt\_project.yml

```yaml
...
models:
  jaffle_shop:
    +materialized: "{{ env_var('DBT_MATERIALIZATION', 'view') }}"
```

This can be useful to avoid compilation errors when the environment variable isn't available.

### Secrets[​](#secrets "Direct link to Secrets")

For certain configurations, you can use "secret" env vars. Any env var named with the prefix `DBT_ENV_SECRET` will be:

* Available for use in `profiles.yml` + `packages.yml`, via the same `env_var()` function
* Disallowed everywhere else, including `dbt_project.yml` and model SQL, to prevent accidentally writing these secret values to the data warehouse or metadata artifacts
* Scrubbed from dbt logs and replaced with `*****`, any time its value appears in those logs (even if the env var was not called directly)

The primary use case of secret env vars is git access tokens for [private packages](https://docs.getdbt.com/docs/build/packages.md#private-packages).

**Note:** When dbt is loading profile credentials and package configuration, secret env vars will be replaced with the string value of the environment variable. You cannot modify secrets using Jinja filters, including type-casting filters such as [`as_number`](https://docs.getdbt.com/reference/dbt-jinja-functions/as_number.md) or [`as_bool`](https://docs.getdbt.com/reference/dbt-jinja-functions/as_bool.md), or pass them as arguments into other Jinja macros. You can only use *one secret* per configuration:

```yml
# works
host: "{{ env_var('DBT_ENV_SECRET_HOST') }}"

# does not work
host: "www.{{ env_var('DBT_ENV_SECRET_HOST_DOMAIN') }}.com/{{ env_var('DBT_ENV_SECRET_HOST_PATH') }}"
```

### Custom metadata[​](#custom-metadata "Direct link to Custom metadata")

Any env var named with the prefix `DBT_ENV_CUSTOM_ENV_` will be included in two places, with its prefix-stripped name as the key:

* [dbt artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md#common-metadata): `metadata` -> `env`
* [events and structured logs](https://docs.getdbt.com/reference/events-logging.md#info-fields): `info` -> `extra`

A dictionary of these prefixed env vars will also be available in a `dbt_metadata_envs` context variable:

```sql
-- {{ dbt_metadata_envs }}

select 1 as id
```

```shell
$ DBT_ENV_CUSTOM_ENV_MY_FAVORITE_COLOR=indigo DBT_ENV_CUSTOM_ENV_MY_FAVORITE_NUMBER=6 dbt compile
```

Compiles to:

```sql
-- {'MY_FAVORITE_COLOR': 'indigo', 'MY_FAVORITE_NUMBER': '6'}

select 1 as id
```

### dbt platform usage[​](#dbt-platform-usage "Direct link to dbt platform usage")

If you are using dbt, you must adhere to the naming conventions for environment variables. Environment variables in dbt must be prefixed with `DBT_` (including `DBT_ENV_CUSTOM_ENV_` or `DBT_ENV_SECRET`). Environment variables keys are uppercased and case sensitive. When referencing `{{env_var('DBT_KEY')}}` in your project's code, the key must match exactly the variable defined in dbt's UI.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
