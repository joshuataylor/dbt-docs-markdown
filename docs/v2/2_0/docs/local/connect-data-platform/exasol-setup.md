# Connect Exasol to dbt Core

* **Maintained by**:
  <!-- -->
  Exasol
* **Authors**:
  <!-- -->
  Torsten Glunde, Ilija Kutle
* **GitHub repo**: [exasol/dbt-exasol](https://github.com/exasol/dbt-exasol) [![](https://img.shields.io/github/stars/exasol/dbt-exasol?style=for-the-badge)](https://github.com/exasol/dbt-exasol)
* **PyPI package**: `dbt-exasol` [![](https://badge.fury.io/py/dbt-exasol.svg)](https://badge.fury.io/py/dbt-exasol)
* **Slack channel**: [n/a]()
* **Supported dbt Core version**:
  <!-- -->
  v1.8.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  Exasol 6.x

## Installing <!-- -->dbt-exasol

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-exasol`

## Configuring <!-- -->dbt-exasol<!-- -->

For <!-- -->Exasol<!-- -->-specific configuration, please refer to [Exasol<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/exasol-configs.md)

### Connecting to Exasol with **dbt-exasol**[​](#connecting-to-exasol-with-dbt-exasol "Direct link to connecting-to-exasol-with-dbt-exasol")

#### User / password authentication[​](#user--password-authentication "Direct link to User / password authentication")

Configure your dbt profile for using Exasol:

##### Exasol connection information[​](#exasol-connection-information "Direct link to Exasol connection information")

profiles.yml

```yaml
dbt-exasol:
  target: dev
  outputs:
    dev:
      type: exasol
      threads: 1
      dsn: HOST:PORT
      user: USERNAME
      password: PASSWORD
      dbname: db
      schema: SCHEMA
```

#### OpenID authentication (Exasol SaaS)[​](#open-id-authentication "Direct link to OpenID authentication (Exasol SaaS)")

For Exasol SaaS environments, you can authenticate using OpenID tokens instead of username and password:

profiles.yml

```yaml
dbt-exasol:
  target: dev
  outputs:
    dev:
      type: exasol
      threads: 1
      dsn: HOST:PORT
      user: USERNAME
      access_token: YOUR_ACCESS_TOKEN  # or use refresh_token
      dbname: db
      schema: SCHEMA
      encryption: True  # required for SaaS
```

* **`access_token`** — Personal access token for OpenID authentication
* **`refresh_token`** — Refresh token for OpenID authentication (alternative to `access_token`)

info

Use either `access_token` or `refresh_token`, not both. TLS encryption is required when using OpenID authentication with Exasol SaaS.

#### Optional parameters[​](#optional-parameters "Direct link to Optional parameters")

* **`connection_timeout`** — defaults to pyexasol default
* **`socket_timeout`** — defaults to pyexasol default
* **`query_timeout`** — defaults to pyexasol default
* **`compression`** — default: False
* **`encryption`** — default: True. Enables SSL/TLS encryption for secure connections. Required for Exasol SaaS
* **`validate_server_certificate`** — default: True. Validates the SSL/TLS certificate when encryption is enabled. Set to False only for development/testing with self-signed certificates (not recommended for production)
* **`protocol_version`** — default: v3
* **`row_separator`** — default: CRLF for windows - LF otherwise
* **`timestamp_format`** — default: `YYYY-MM-DDTHH:MI:SS.FF6`

SSL/TLS Certificate Validation

By default, dbt-exasol validates SSL/TLS certificates when `encryption=True`. For development/testing with self-signed certificates, you can either:

* Set `validate_server_certificate: False` (not recommended for production)
* Use a certificate fingerprint in the DSN: `dsn: myhost/FINGERPRINT:8563`
* Use `dsn: myhost/nocertcheck:8563` to skip validation (testing only)

For more information, see the [PyExasol security documentation](https://exasol.github.io/pyexasol/master/user_guide/configuration/security.html).
