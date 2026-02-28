# IBM DB2 setup

Community plugin

Some core functionality may be limited. If you're interested in contributing, check out the source code for each repository listed below.

<!-- -->

* **Maintained by**:
  <!-- -->
  Community
* **Authors**:
  <!-- -->
  Rasmus Nyberg (https\://github.com/aurany)
* **GitHub repo**: [aurany/dbt-ibmdb2](https://github.com/aurany/dbt-ibmdb2) [![](https://img.shields.io/github/stars/aurany/dbt-ibmdb2?style=for-the-badge)](https://github.com/aurany/dbt-ibmdb2)
* **PyPI package**: `dbt-ibmdb2` [![](https://badge.fury.io/py/dbt-ibmdb2.svg)](https://badge.fury.io/py/dbt-ibmdb2)
* **Slack channel**: [n/a](https://www.getdbt.com/community)
* **Supported dbt Core version**:
  <!-- -->
  v1.0.4
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  IBM DB2 V9fp2

## Installing <!-- -->dbt-ibmdb2

Use `pip` to install the adapter. Before 1.8, installing the adapter would automatically install `dbt-core` and any additional dependencies. Beginning in 1.8, installing an adapter does not automatically install `dbt-core`. This is because adapters and dbt Core versions have been decoupled from each other so we no longer want to overwrite existing dbt-core installations. Use the following command for installation:

`python -m pip install dbt-core dbt-ibmdb2`

## Configuring <!-- -->dbt-ibmdb2<!-- -->

For <!-- -->IBM DB2<!-- -->-specific configuration, please refer to [IBM DB2<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/no-configs.md)

This is an experimental plugin:

* We have not tested it extensively
* Tested with [dbt-adapter-tests](https://pypi.org/project/pytest-dbt-adapter/) and DB2 LUW on Mac OS+RHEL8
* Compatibility with other [dbt packages](https://hub.getdbt.com/) (like [dbt\_utils](https://hub.getdbt.com/dbt-labs/dbt_utils/latest/)) is only partially tested

## Connecting to IBM DB2 with dbt-ibmdb2[​](#connecting-to-ibm-db2-with-dbt-ibmdb2 "Direct link to Connecting to IBM DB2 with dbt-ibmdb2")

IBM DB2 targets should be set up using the following configuration in your `profiles.yml` file.

Example:

\~/.dbt/profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: ibmdb2
      schema: analytics
      database: test
      host: localhost
      port: 50000
      protocol: TCPIP
      username: my_username
      password: my_password
```

#### Description of IBM DB2 Profile Fields[​](#description-of-ibm-db2-profile-fields "Direct link to Description of IBM DB2 Profile Fields")

| Option   | Description                                          | Required? | Example       |
| -------- | ---------------------------------------------------- | --------- | ------------- |
| type     | The specific adapter to use                          | Required  | `ibmdb2`      |
| schema   | Specify the schema (database) to build models into   | Required  | `analytics`   |
| database | Specify the database you want to connect to          | Required  | `testdb`      |
| host     | Hostname or IP-address                               | Required  | `localhost`   |
| port     | The port to use                                      | Optional  | `50000`       |
| protocol | Protocol to use                                      | Optional  | `TCPIP`       |
| username | The username to use to connect to the server         | Required  | `my-username` |
| password | The password to use for authenticating to the server | Required  | `my-password` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Supported features[​](#supported-features "Direct link to Supported features")

| DB2 LUW | DB2 z/OS | Feature                     |
| ------- | -------- | --------------------------- |
| ✅      | 🤷       | Table materialization       |
| ✅      | 🤷       | View materialization        |
| ✅      | 🤷       | Incremental materialization |
| ✅      | 🤷       | Ephemeral materialization   |
| ✅      | 🤷       | Seeds                       |
| ✅      | 🤷       | Sources                     |
| ✅      | 🤷       | Custom data tests           |
| ✅      | 🤷       | Docs generate               |
| ✅      | 🤷       | Snapshots                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Notes[​](#notes "Direct link to Notes")

* dbt-ibmdb2 is built on the ibm\_db python package and there are some known encoding issues related to z/OS.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
