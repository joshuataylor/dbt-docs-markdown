# Extrica Setup

## Overview of <!-- -->dbt-extrica<!-- -->

* **Maintained by**:
  <!-- -->
  Extrica, Trianz
* **Authors**:
  <!-- -->
  Gaurav Mittal, Viney Kumar, Mohammed Feroz, and Mrinal Mayank
* **GitHub repo**: [extricatrianz/dbt-extrica](https://github.com/extricatrianz/dbt-extrica)
* **PyPI package**: `dbt-extrica` [![](https://badge.fury.io/py/dbt-extrica.svg)](https://badge.fury.io/py/dbt-extrica)
* **Supported dbt Core version**:
  <!-- -->
  v1.7.2
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  n/a

## Installing <!-- -->dbt-extrica<!-- -->

Use `pip` to install the adapter, which automatically installs `dbt-core` and any additional dependencies. Use the following command for installation:

`python -m pip install dbt-extrica`

## Connecting to <!-- -->Extrica<!-- -->

#### Example profiles.yml[​](#example-profilesyml "Direct link to Example profiles.yml")

Here is an example of dbt-extrica profiles. At a minimum, you need to specify `type`, `method`, `username`, `password` `host`, `port`, `schema`, `catalog` and `threads`.

\~/.dbt/profiles.yml

```yaml
<profile-name>:
  outputs:
    dev:
      type: extrica
      method: jwt 
      username: [username for jwt auth]
      password: [password for jwt auth]  
      host: [extrica hostname]
      port: [port number]
      schema: [dev_schema]
      catalog: [catalog_name]
      threads: [1 or more]

    prod:
      type: extrica
      method: jwt 
      username: [username for jwt auth]
      password: [password for jwt auth]  
      host: [extrica hostname]
      port: [port number]
      schema: [dev_schema]
      catalog: [catalog_name]
      threads: [1 or more]
  target: dev
```

#### Description of Extrica Profile Fields[​](#description-of-extrica-profile-fields "Direct link to Description of Extrica Profile Fields")

| Parameter | Type    | Description                                                                                                          |
| --------- | ------- | -------------------------------------------------------------------------------------------------------------------- |
| type      | string  | Specifies the type of dbt adapter (Extrica).                                                                         |
| method    | jwt     | Authentication method for JWT authentication.                                                                        |
| username  | string  | Username for JWT authentication. The obtained JWT token is used to initialize a trino.auth.JWTAuthentication object. |
| password  | string  | Password for JWT authentication. The obtained JWT token is used to initialize a trino.auth.JWTAuthentication object. |
| host      | string  | The host parameter specifies the hostname or IP address of the Extrica's Trino server.                               |
| port      | integer | The port parameter specifies the port number on which the Extrica's Trino server is listening.                       |
| schema    | string  | Schema or database name for the connection.                                                                          |
| catalog   | string  | Name of the catalog representing the data source.                                                                    |
| threads   | integer | Number of threads for parallel execution of queries. (1 or more)                                                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
