# Connect MindsDB to dbt Core

Local developmentⓘ

Vendor-supported plugin

The dbt-mindsdb package allows dbt to connect to [MindsDB](https://github.com/mindsdb/mindsdb).

<!-- -->

* **Maintained by**:
  <!-- -->
  MindsDB
* **Authors**:
  <!-- -->
  MindsDB team
* **GitHub repo**: [mindsdb/dbt-mindsdb](https://github.com/mindsdb/dbt-mindsdb) [![](https://img.shields.io/github/stars/mindsdb/dbt-mindsdb?style=for-the-badge)](https://github.com/mindsdb/dbt-mindsdb)
* **PyPI package**: `dbt-mindsdb` [![](https://badge.fury.io/py/dbt-mindsdb.svg)](https://badge.fury.io/py/dbt-mindsdb)
* **Slack channel**: [n/a](https://www.getdbt.com/community)
* **Supported dbt Core version**:
  <!-- -->
  v1.0.1
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  ?

## Installing <!-- -->dbt-mindsdb

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-mindsdb`

## Configuring <!-- -->dbt-mindsdb<!-- -->

For <!-- -->MindsDB<!-- -->-specific configuration, please refer to [MindsDB<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/mindsdb-configs.md)

## Configurations[​](#configurations "Direct link to Configurations")

Basic `profiles.yml` for connecting to MindsDB:

```yml
mindsdb:
  outputs:
    dev:
      database: 'mindsdb'
      host: '127.0.0.1'
      password: ''
      port: 47335
      schema: 'mindsdb'
      type: mindsdb
      username: 'mindsdb'
  target: dev
```

| Key      | Required | Description                                          | Example                         |
| -------- | -------- | ---------------------------------------------------- | ------------------------------- |
| type     | ✔️       | The specific adapter to use                          | `mindsdb`                       |
| host     | ✔️       | The MindsDB (hostname) to connect to                 | `cloud.mindsdb.com`             |
| port     | ✔️       | The port to use                                      | `3306` or `47335`               |
| schema   | ✔️       | Specify the schema (database) to build models into   | The MindsDB datasource          |
| username | ✔️       | The username to use to connect to the server         | `mindsdb` or mindsdb cloud user |
| password | ✔️       | The password to use for authenticating to the server | \`pass                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |
