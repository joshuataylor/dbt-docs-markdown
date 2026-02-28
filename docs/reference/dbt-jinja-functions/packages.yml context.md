# About packages.yml context

The following context methods and variables are available when configuring a `packages.yml` file.

**Available context methods:**

* [env\_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md)
  * Use `env_var()` in any dbt YAML file that supports Jinja. Only `packages.yml` and `profiles.yml` support environment variables for [secure values](https://docs.getdbt.com/docs/build/dbt-tips.md#yaml-tips) (using the `DBT_ENV_SECRET_` prefix).
* [var](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md) (Note: only variables defined with `--vars` are available. Refer to [YAML tips](https://docs.getdbt.com/docs/build/dbt-tips.md#yaml-tips) for more information)

**Available context variables:**

* [builtins](https://docs.getdbt.com/reference/dbt-jinja-functions/builtins.md)
* [dbt\_version](https://docs.getdbt.com/reference/dbt-jinja-functions/dbt_version.md)
* [target](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md)

## Example usage[​](#example-usage "Direct link to Example usage")

The following examples show how to use the different context methods and variables in your `packages.yml`.

Use `builtins` in your `packages.yml`:

```text
packages:
  - package: dbt-labs/dbt_utils
    version: "{% if builtins is defined %}0.14.0{% else %}0.13.1{% endif %}"
```

Use `env_var` in your `packages.yml`:

```text
packages:
  - package: dbt-labs/dbt_utils
    version: "{{ env_var('DBT_UTILS_VERSION') }}"
```

Use `dbt_version` in your `packages.yml`:

```text
packages:
  - package: dbt-labs/dbt_utils
    version: "{% if dbt_version is defined %}0.14.0{% else %}0.13.1{% endif %}"
```

Use `target` in your `packages.yml`:

```text

packages:
  - package: dbt-labs/dbt_utils
    version: "{% if target.name == 'prod' %}0.14.0{% else %}0.13.1{% endif %}"
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Packages](https://docs.getdbt.com/docs/build/packages.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
