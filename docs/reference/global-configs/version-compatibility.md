# Checking version compatibility

For the first several years of dbt Core's development, breaking changes were more common. For this reason, we encouraged setting [dbt version requirements](https://docs.getdbt.com/reference/project-configs/require-dbt-version.md) — especially if they use features that are newer or which may break in future versions of dbt Core. By default, if you run a project with an incompatible dbt version, dbt will raise an error.

You can use the `VERSION_CHECK` config to disable this check and suppress the error message:

```text
$ dbt run --no-version-check
Running with dbt=1.0.0
Found 13 models, 2 tests, 1 archives, 0 analyses, 204 macros, 2 operations....
```

dbt release tracks

Starting in 2024, when you select a [release track in dbt](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) to receive ongoing dbt version upgrades, dbt will ignore the `require-dbt-version` config.

dbt Labs is committed to zero breaking changes for code in dbt projects, with ongoing releases to dbt and new versions of dbt Core. We also recommend these best practices:

 Installing dbt packages

If you install dbt packages for use in your project, whether the package is maintained by your colleagues or a member of the open source dbt community, we recommend pinning the package to a specific revision or `version` boundary. dbt manages this out-of-the-box by *locking* the version/revision of packages in development in order to guarantee predictable builds in production. To learn more, refer to [Predictable package installs](https://docs.getdbt.com/reference/commands/deps.md#predictable-package-installs).

 Maintaining dbt packages

If you maintain dbt packages, whether on behalf of your colleagues or members of the open source community, we recommend writing defensive code that checks to verify that other required packages and global macros are available. For example, if your package depends on the availability of a `date_spine` macro in the global `dbt` namespace, you can write:

models/some\_days.sql

```sql
{% macro a_few_days_in_september() %}

    {% if not dbt.get('date_spine') %}
      {{ exceptions.raise_compiler_error("Expected to find the dbt.date_spine macro, but it could not be found") }}
    {% endif %}

    {{ date_spine("day", "cast('2020-01-01' as date)", "cast('2030-12-31' as date)") }}

{% endmacro %}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
