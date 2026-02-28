# Greenplum setup

* **Maintained by**:
  <!-- -->
  Community
* **Authors**:
  <!-- -->
  Mark Poroshin, Dmitry Bevz
* **GitHub repo**: [markporoshin/dbt-greenplum](https://github.com/markporoshin/dbt-greenplum) [![](https://img.shields.io/github/stars/markporoshin/dbt-greenplum?style=for-the-badge)](https://github.com/markporoshin/dbt-greenplum)
* **PyPI package**: `dbt-greenplum` [![](https://badge.fury.io/py/dbt-greenplum.svg)](https://badge.fury.io/py/dbt-greenplum)
* **Slack channel**: [n/a](https://www.getdbt.com/community)
* **Supported dbt Core version**:
  <!-- -->
  v1.0.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  Greenplum 6.0

## Installing <!-- -->dbt-greenplum

Use `pip` to install the adapter. Before 1.8, installing the adapter would automatically install `dbt-core` and any additional dependencies. Beginning in 1.8, installing an adapter does not automatically install `dbt-core`. This is because adapters and dbt Core versions have been decoupled from each other so we no longer want to overwrite existing dbt-core installations. Use the following command for installation:

`python -m pip install dbt-core dbt-greenplum`

## Configuring <!-- -->dbt-greenplum<!-- -->

For <!-- -->Greenplum<!-- -->-specific configuration, please refer to [Greenplum<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/greenplum-configs.md)

For further (and more likely up-to-date) info, see the [README](https://github.com/markporoshin/dbt-greenplum#README.md)

## Profile Configuration[​](#profile-configuration "Direct link to Profile Configuration")

Greenplum targets should be set up using the following configuration in your `profiles.yml` file.

\~/.dbt/profiles.yml

```yaml
company-name:
  target: dev
  outputs:
    dev:
      type: greenplum
      host: [hostname]
      user: [username]
      password: [password]
      port: [port]
      dbname: [database name]
      schema: [dbt schema]
      threads: [1 or more]
      keepalives_idle: 0 # default 0, indicating the system default. See below
      connect_timeout: 10 # default 10 seconds
      search_path: [optional, override the default postgres search_path]
      role: [optional, set the role dbt assumes when executing queries]
      sslmode: [optional, set the sslmode used to connect to the database]
```

### Notes[​](#notes "Direct link to Notes")

This adapter strongly depends on dbt-postgres, so you can read more about configurations here [Profile Setup](https://docs.getdbt.com/docs/core/connect-data-platform/postgres-setup.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
