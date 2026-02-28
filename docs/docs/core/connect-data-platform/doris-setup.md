# Doris setup

* **Maintained by**:
  <!-- -->
  SelectDB
* **Authors**:
  <!-- -->
  catpineapple,JNSimba
* **GitHub repo**: [selectdb/dbt-doris](https://github.com/selectdb/dbt-doris) [![](https://img.shields.io/github/stars/selectdb/dbt-doris?style=for-the-badge)](https://github.com/selectdb/dbt-doris)
* **PyPI package**: `dbt-doris` [![](https://badge.fury.io/py/dbt-doris.svg)](https://badge.fury.io/py/dbt-doris)
* **Slack channel**: [#db-doris](https://www.getdbt.com/community)
* **Supported dbt Core version**:
  <!-- -->
  v1.3.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:

## Installing <!-- -->dbt-doris

Use `pip` to install the adapter. Before 1.8, installing the adapter would automatically install `dbt-core` and any additional dependencies. Beginning in 1.8, installing an adapter does not automatically install `dbt-core`. This is because adapters and dbt Core versions have been decoupled from each other so we no longer want to overwrite existing dbt-core installations. Use the following command for installation:

`python -m pip install dbt-core dbt-doris`

## Configuring <!-- -->dbt-doris<!-- -->

For <!-- -->Apache Doris / SelectDB<!-- -->-specific configuration, please refer to [Apache Doris / SelectDB<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/doris-configs.md)

## Connecting to Doris/SelectDB with **dbt-doris**[​](#connecting-to-dorisselectdb-with-dbt-doris "Direct link to connecting-to-dorisselectdb-with-dbt-doris")

### User / Password Authentication[​](#user--password-authentication "Direct link to User / Password Authentication")

Configure your dbt profile for using Doris:

#### Doris connection profile[​](#doris-connection-profile "Direct link to Doris connection profile")

profiles.yml

```yaml
dbt-doris:
  target: dev
  outputs:
    dev:
      type: doris
      host: 127.0.0.1
      port: 9030
      schema: database_name
      username: username
      password: password
```

#### Description of Profile Fields[​](#description-of-profile-fields "Direct link to Description of Profile Fields")

| Option   | Description                                                                                                                      | Required? | Example     |
| -------- | -------------------------------------------------------------------------------------------------------------------------------- | --------- | ----------- |
| type     | The specific adapter to use                                                                                                      | Required  | `doris`     |
| host     | The hostname to connect to                                                                                                       | Required  | `127.0.0.1` |
| port     | The port to use                                                                                                                  | Required  | `9030`      |
| schema   | Specify the schema (database) to build models into, doris have not schema to make a collection of table or view' like PostgreSql | Required  | `dbt`       |
| username | The username to use to connect to the doris                                                                                      | Required  | `root`      |
| password | The password to use for authenticating to the doris                                                                              | Required  | `password`  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Database User Privileges[​](#database-user-privileges "Direct link to Database User Privileges")

Your Doris/SelectDB database user would be able to have some abilities to read or write. You can find some help [here](https://doris.apache.org/docs/admin-manual/privilege-ldap/user-privilege) with Doris privileges management.

| Required Privilege |
| ------------------ |
| Select\_priv       |
| Load\_priv         |
| Alter\_priv        |
| Create\_priv       |
| Drop\_priv         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
