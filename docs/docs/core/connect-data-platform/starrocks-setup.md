# Starrocks setup

## Overview of <!-- -->dbt-starrocks<!-- -->

* **Maintained by**:
  <!-- -->
  Starrocks
* **Authors**:
  <!-- -->
  Astralidea
* **GitHub repo**: [StarRocks/dbt-starrocks](https://github.com/StarRocks/dbt-starrocks)[![](https://img.shields.io/github/stars/StarRocks/dbt-starrocks?style=for-the-badge)](https://github.com/StarRocks/dbt-starrocks)
* **PyPI package**: `dbt-starrocks` [![](https://badge.fury.io/py/dbt-starrocks.svg)](https://badge.fury.io/py/dbt-starrocks)
* **Slack channel**: [#db-starrocks](https://www.getdbt.com/community)
* **Supported dbt Core version**:
  <!-- -->
  v1.6.2
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  Starrocks 2.5

## Installing <!-- -->dbt-starrocks<!-- -->

pip is the easiest way to install the adapter:

`python -m pip install dbt-starrocks`

Installing `dbt-starrocks` will also install `dbt-core` and any other dependencies.

## Configuring <!-- -->dbt-starrocks<!-- -->

For <!-- -->Starrocks<!-- -->-specifc configuration please refer to [Starrocks<!-- --> Configuration](https://docs.getdbt.com/reference/resource-configs/starrocks-configs.md)

For further info, refer to the GitHub repository: [StarRocks/dbt-starrocks](https://github.com/StarRocks/dbt-starrocks)

## Authentication Methods[​](#authentication-methods "Direct link to Authentication Methods")

### User / Password Authentication[​](#user--password-authentication "Direct link to User / Password Authentication")

Starrocks can be configured using basic user/password authentication as shown below.

\~/.dbt/profiles.yml

```yaml
my-starrocks-db:
  target: dev
  outputs:
    dev:
      type: starrocks
      host: localhost
      port: 9030
      schema: analytics
      
      # User/password auth
      username: your_starrocks_username
      password: your_starrocks_password
```

#### Description of Profile Fields[​](#description-of-profile-fields "Direct link to Description of Profile Fields")

| Option   | Description                                            | Required? | Example                        |
| -------- | ------------------------------------------------------ | --------- | ------------------------------ |
| type     | The specific adapter to use                            | Required  | `starrocks`                    |
| host     | The hostname to connect to                             | Required  | `192.168.100.28`               |
| port     | The port to use                                        | Required  | `9030`                         |
| schema   | Specify the schema (database) to build models into     | Required  | `analytics`                    |
| username | The username to use to connect to the server           | Required  | `dbt_admin`                    |
| password | The password to use for authenticating to the server   | Required  | `correct-horse-battery-staple` |
| version  | Let Plugin try to go to a compatible starrocks version | Optional  | `3.1.0`                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Supported features[​](#supported-features "Direct link to Supported features")

| Starrocks <= 2.5 | Starrocks 2.5 \~ 3.1 | Starrocks >= 3.1 | Feature                           |
| ---------------- | -------------------- | ---------------- | --------------------------------- |
| ✅               | ✅                   | ✅               | Table materialization             |
| ✅               | ✅                   | ✅               | View materialization              |
| ❌               | ❌                   | ✅               | Materialized View materialization |
| ❌               | ✅                   | ✅               | Incremental materialization       |
| ❌               | ✅                   | ✅               | Primary Key Model                 |
| ✅               | ✅                   | ✅               | Sources                           |
| ✅               | ✅                   | ✅               | Custom data tests                 |
| ✅               | ✅                   | ✅               | Docs generate                     |
| ❌               | ❌                   | ❌               | Kafka                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Notice[​](#notice "Direct link to Notice")

1. When StarRocks Version < 2.5, `Create table as` can only set engine='OLAP' and table\_type='DUPLICATE'
2. When StarRocks Version >= 2.5, `Create table as` supports table\_type='PRIMARY'
3. When StarRocks Version < 3.1 distributed\_by is required

It is recommended to use the latest starrocks version and dbt-starrocks version for the best experience.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
