# Upsolver setup

## Overview of <!-- -->dbt-upsolver<!-- -->

* **Maintained by**:
  <!-- -->
  Upsolver Team
* **Authors**:
  <!-- -->
  Upsolver Team
* **GitHub repo**: [Upsolver/dbt-upsolver](https://github.com/Upsolver/dbt-upsolver)[![](https://img.shields.io/github/stars/Upsolver/dbt-upsolver?style=for-the-badge)](https://github.com/Upsolver/dbt-upsolver)
* **PyPI package**: `dbt-upsolver` [![](https://badge.fury.io/py/dbt-upsolver.svg)](https://badge.fury.io/py/dbt-upsolver)
* **Slack channel**: [Upsolver Community](https://join.slack.com/t/upsolvercommunity/shared_invite/zt-1zo1dbyys-hj28WfaZvMh4Z4Id3OkkhA)
* **Supported dbt Core version**:
  <!-- -->
  v1.5.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  n/a

## Installing <!-- -->dbt-upsolver<!-- -->

pip is the easiest way to install the adapter:

`python -m pip install dbt-upsolver`

Installing `dbt-upsolver` will also install `dbt-core` and any other dependencies.

## Configuring <!-- -->dbt-upsolver<!-- -->

For <!-- -->Upsolver<!-- -->-specifc configuration please refer to [Upsolver<!-- --> Configuration](https://docs.getdbt.com/reference/resource-configs/upsolver-configs.md)

For further info, refer to the GitHub repository: [Upsolver/dbt-upsolver](https://github.com/Upsolver/dbt-upsolver)

## Authentication Methods[​](#authentication-methods "Direct link to Authentication Methods")

### User / Token authentication[​](#user--token-authentication "Direct link to User / Token authentication")

Upsolver can be configured using basic user/token authentication as shown below.

\~/.dbt/profiles.yml

```yaml
my-upsolver-db:
  target: dev
  outputs:
    dev:
      type: upsolver
      api_url: https://mt-api-prod.upsolver.com

      user: [username]
      token: [token]

      database: [database name]
      schema: [schema name]
      threads: [1 or more]
```

## Configurations[​](#configurations "Direct link to Configurations")

The configs for Upsolver targets are shown below.

### All configurations[​](#all-configurations "Direct link to All configurations")

| Config   | Required? | Description                                                                                                |
| -------- | --------- | ---------------------------------------------------------------------------------------------------------- |
| token    | Yes       | The token to connect Upsolver [Upsolver's documentation](https://docs.upsolver.com/sqlake/api-integration) |
| user     | Yes       | The user to log in as                                                                                      |
| database | Yes       | The database that dbt should create models in                                                              |
| schema   | Yes       | The schema to build models into by default                                                                 |
| api\_url | Yes       | The API url to connect. Common value `https://mt-api-prod.upsolver.com`                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
