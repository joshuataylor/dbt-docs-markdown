# Connect Snowflake to dbt Core

Snowflake enforcing strong authentication

Starting August 31, 2026, password authentication will no longer be supported. Please update your environments to use key-pair or OAuth by that date to prevent service disruptions.

[Fusion compatible](https://docs.getdbt.com/docs/local/connect-data-platform/snowflake-setup.md?version=2 "Fusion compatible") connection also available.

* **Maintained by**:
  <!-- -->
  dbt Labs
* **Authors**:
  <!-- -->
  dbt maintainers
* **GitHub repo**: [dbt-labs/dbt-adapters](https://github.com/dbt-labs/dbt-adapters) [![](https://img.shields.io/github/stars/dbt-labs/dbt-adapters?style=for-the-badge)](https://github.com/dbt-labs/dbt-adapters)
* **PyPI package**: `dbt-snowflake` [![](https://badge.fury.io/py/dbt-snowflake.svg)](https://badge.fury.io/py/dbt-snowflake)
* **Slack channel**: [#db-snowflake](https://getdbt.slack.com/archives/C01DRQ178LQ)
* **Supported dbt Core version**:
  <!-- -->
  v0.8.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Supported
* **Minimum data platform version**:
  <!-- -->
  n/a

## Installing <!-- -->dbt-snowflake

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-snowflake`

## Configuring <!-- -->dbt-snowflake<!-- -->

For <!-- -->Snowflake<!-- -->-specific configuration, please refer to [Snowflake<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md)

Snowflake column size change

[Snowflake plans to increase](https://docs.snowflake.com/en/release-notes/bcr-bundles/un-bundled/bcr-2118) the default column size for string and binary data types in September 2026. `dbt-snowflake` versions below v1.10.6 may fail to build certain incremental models when this change is deployed.

 Assess impact and required actions

If you're using a `dbt-snowflake` version below v1.10.6 or have not yet migrated to a [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) in the dbt platform, your adapter version is incompatible with this change and may fail to build incremental models that meet *both* of the following conditions:

* Contain string columns with collation defined
* Use the `on_schema_change='sync_all_columns'` config

To check whether this change affects your project, run the following [list](https://docs.getdbt.com/reference/commands/list.md) command:

```bash
dbt ls -s config.materialized:incremental,config.on_schema_change:sync_all_columns --resource-type model
```

* If the command returns `No nodes selected!`, no action is required.

* If the command returns one or more models (for example, `Found 1000 models, 644 macros`), you may be impacted if those models have string columns that don't specify a width. In that case, upgrade to a version that includes the fix:

  * **dbt Core**: `dbt-snowflake` v1.10.6 or later. For upgrade instructions, refer to [Upgrade adapters](https://docs.getdbt.com/docs/local/install-dbt.md) in the dbt Core v1 installation instructions.
  * **dbt platform**: Any release track (Latest, Compatible, Extended, or Fallback).
  * **dbt Fusion engine**: v2.0.0-preview\.147 or higher.

  This ensures your incremental models can safely handle schema changes while maintaining required collation settings.

## Authentication Methods[​](#authentication-methods "Direct link to Authentication Methods")

### User / Password authentication[​](#user--password-authentication "Direct link to User / Password authentication")

Snowflake can be configured using basic user/password authentication as shown below.

\~/.dbt/profiles.yml

```yaml
my-snowflake-db:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: [account id]

      # User/password auth
      user: [username]
      password: [password]

      role: [user role]
      database: [database name]
      warehouse: [warehouse name]
      schema: [dbt schema]
      threads: [1 or more]
      client_session_keep_alive: False
      query_tag: [anything]

      # optional
      connect_retries: 0 # default 0
      connect_timeout: 10 # default: 10
      retry_on_database_errors: True # default: true
      retry_all: False  # default: false
      reuse_connections: True # default: True if client_session_keep_alive is False, otherwise None
```

### User / Password + DUO MFA authentication[​](#user--password--duo-mfa-authentication "Direct link to User / Password + DUO MFA authentication")

Snowflake integrates the DUO Mobile app to add 2-Factor authentication to basic user/password as seen below.

```yaml
my-snowflake-db:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: [account id]

      # User/password auth
      user: [username]
      password: [password]
      authenticator: username_password_mfa

      role: [user role]
      database: [database name]
      warehouse: [warehouse name]
      schema: [dbt schema]
      threads: [1 or more]
      client_session_keep_alive: False
      query_tag: [anything]

      # optional
      connect_retries: 0 # default 0
      connect_timeout: 10 # default: 10
      retry_on_database_errors: True # default: true
      retry_all: False  # default: false
      reuse_connections: True # default: True if client_session_keep_alive is False, otherwise None
```

**Note:** To avoid receiving Duo push notifications for every model build, enable [MFA token caching](https://docs.snowflake.com/en/user-guide/security-mfa#label-mfa-token-caching) in your Snowflake warehouse by running `alter account set allow_client_mfa_caching = true;` with the ACCOUNTADMIN role.

### Key pair authentication[​](#key-pair-authentication "Direct link to Key pair authentication")

To use key pair authentication, specify the `private_key_path` in your configuration, avoiding the use of a `password`. If needed, you can add a `private_key_passphrase`. **Note**: Unencrypted private keys are accepted, so add a passphrase only if necessary. However, for dbt Core versions 1.5 and 1.6, configurations using a private key in PEM format (for example, keys enclosed with BEGIN and END tags) are not supported. In these versions, you must use the `private_key_path` to reference the location of your private key file.

dbt can specify a `private_key` directly as a string instead of a `private_key_path`. This `private_key` string can be in either Base64-encoded DER format, representing the key bytes, or in plain-text PEM format. Refer to [Snowflake documentation](https://docs.snowflake.com/en/user-guide/key-pair-auth) for more info on how they generate the key.

\~/.dbt/profiles.yml

```yaml
my-snowflake-db:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: [account id]
      user: [username]
      role: [user role]

      # Keypair config
      # For dbt Fusion engine, make sure to read requirements about using PKCS#8 format with AES-256 encryption in the following section.
      private_key_path: [path/to/private.key]
      # or private_key instead of private_key_path
      private_key_passphrase: [passphrase for the private key, if key is encrypted]

      database: [database name]
      warehouse: [warehouse name]
      schema: [dbt schema]
      threads: [1 or more]
      client_session_keep_alive: False
      query_tag: [anything]

      # optional
      connect_retries: 0 # default 0
      connect_timeout: 10 # default: 10
      retry_on_database_errors: True # default: true
      retry_all: False  # default: false
      reuse_connections: True # default: True if client_session_keep_alive is False, otherwise None
```

#### dbt Fusion engine key formats[​](#dbt-fusion-engine-key-formats "Direct link to dbt Fusion engine key formats")

Fusion requires modern key formats and doesn't support legacy 3DES encryption or headerless keys. We recommend using PKCS#8 format with AES-256 encryption for key pair authentication with Fusion. Using older key formats may cause authentication failures.

If you encounter the `Key is PKCS#1 (RSA private key). Snowflake requires PKCS#8` error, then your private key is in the wrong format. You have two options:

* (Recommended fix) Re-export your key with modern encryption:

  ```bash
  # Convert to PKCS#8 with AES-256 encryption
  openssl genrsa 2048 | openssl pkcs8 -topk8 -v2 aes-256-cbc -inform PEM -out rsa_key.p8
  ```

* (Temporary workaround) Add the `BEGIN` header and `END` footer to your PEM body:

  ```bash
  -----BEGIN ENCRYPTED PRIVATE KEY-----
  < your existing encrypted private key contents >
  -----END ENCRYPTED PRIVATE KEY-----
  ```

### SSO authentication[​](#sso-authentication "Direct link to SSO authentication")

To use SSO authentication for Snowflake, omit a `password` and instead supply an `authenticator` config set to 'externalbrowser' to your target.

Refer to the following example:

\~/.dbt/profiles.yml

```yaml
my-snowflake-db:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: [account id] # Snowflake <account_name>
      user: [username] # Snowflake username
      role: [user role] # Snowflake user role

      # SSO config
      authenticator: externalbrowser

      database: [database name] # Snowflake database name
      warehouse: [warehouse name] # Snowflake warehouse name
      schema: [dbt schema]
      threads: [between 1 and 8]
      client_session_keep_alive: False
      query_tag: [anything]

      # optional
      connect_retries: 0 # default 0
      connect_timeout: 10 # default: 10
      retry_on_database_errors: True # default: true
      retry_all: False  # default: false
      reuse_connections: True # default: True if client_session_keep_alive is False, otherwise None
```

**Note**: To avoid authentication prompts for every dbt connection (which can result in dozens of SSO tabs opening), enable [connection caching](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-use#using-connection-caching-to-minimize-the-number-of-prompts-for-authentication-optional) in your Snowflake warehouse by running `alter account set allow_id_token = true;` with the ACCOUNTADMIN role.

### OAuth authorization[​](#oauth-authorization "Direct link to OAuth authorization")

To learn how to configure OAuth in Snowflake, refer to their [documentation](https://docs.snowflake.com/en/user-guide/oauth-snowflake-overview). Your Snowflake admin needs to generate an [OAuth token](https://community.snowflake.com/s/article/HOW-TO-OAUTH-TOKEN-GENERATION-USING-SNOWFLAKE-CUSTOM-OAUTH) for your configuration to work.

Provide the OAUTH\_REDIRECT\_URI in Snowflake:`http://localhost:PORT_NUMBER`. For example, `http://localhost:8080`.

Once your Snowflake admin has configured OAuth, add the following to your `profiles.yml` file:

```yaml

my-snowflake-db:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: [account id]
      
      # The following fields are retrieved from the Snowflake configuration
      authenticator: oauth
      oauth_client_id: [OAuth client id]
      oauth_client_secret: [OAuth client secret]
      token: [OAuth refresh token]
```

## Configurations[​](#configurations "Direct link to Configurations")

The "base" configs for Snowflake targets are shown below. Note that you should also specify auth-related configs specific to the authentication method you are using as described above.

### All configurations[​](#all-configurations "Direct link to All configurations")

| Config                                | Required?            | Description                                                                                                                                                                                                                                                                  |
| ------------------------------------- | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| account                               | Yes                  | The account to connect to as per [Snowflake's documentation](https://docs.snowflake.com/en/user-guide/intro-regions.html#specifying-region-information-in-your-account-hostname). See notes [below](#account)                                                                |
| user                                  | Yes                  | The user to log in as                                                                                                                                                                                                                                                        |
| database                              | Yes                  | The database that dbt should create models in                                                                                                                                                                                                                                |
| warehouse                             | Yes                  | The warehouse to use when building models                                                                                                                                                                                                                                    |
| schema                                | Yes                  | The schema to build models into by default. Can be overridden with [custom schemas](https://docs.getdbt.com/docs/build/custom-schemas.md)                                                                                                                                    |
| role                                  | No (but recommended) | The role to assume when running queries as the specified user.                                                                                                                                                                                                               |
| client\_session\_keep\_alive          | No                   | If `True`, the snowflake client will keep connections for longer than the default 4 hours. This is helpful when particularly long-running queries are executing (> 4 hours). Default: False (see [note below](#client_session_keep_alive))                                   |
| threads                               | No                   | The number of concurrent models dbt should build. Set this to a higher number if using a bigger warehouse. Default=1                                                                                                                                                         |
| query\_tag                            | No                   | A value with which to tag all queries, for later searching in [QUERY\_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/query_history.html)                                                                                                            |
| retry\_all                            | No                   | A boolean flag indicating whether to retry on all [Snowflake connector errors](https://github.com/snowflakedb/snowflake-connector-python/blob/main/src/snowflake/connector/errors.py)                                                                                        |
| retry\_on\_database\_errors           | No                   | A boolean flag indicating whether to retry after encountering errors of type [snowflake.connector.errors.DatabaseError](https://github.com/snowflakedb/snowflake-connector-python/blob/ffdd6b3339aa71885878d047141fe9a77c4a4ae3/src/snowflake/connector/errors.py#L361-L364) |
| connect\_retries                      | No                   | The number of times to retry after an unsuccessful connection                                                                                                                                                                                                                |
| connect\_timeout                      | No                   | The number of seconds to sleep between failed connection retries                                                                                                                                                                                                             |
| reuse\_connections                    | No                   | A boolean flag indicating whether to reuse idle connections to help reduce total connections opened. Default is `True` when `client_session_keep_alive` is `False` or unset; otherwise `None` (must be explicitly set).                                                      |
| platform\_detection\_timeout\_seconds | No                   | Timeout (in seconds) for platform detection. Defaults to `0.0`. Set to a positive value if using Workload Identity Federation (WIF) authentication.                                                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### account[​](#account "Direct link to account")

For AWS accounts in the US West default region, you can use `abc123` (without any other segments). For some AWS accounts you will have to append the region and/or cloud platform. For example, `abc123.eu-west-1` or `abc123.eu-west-2.aws`.

For GCP and Azure-based accounts, you have to append the region and cloud platform, such as `gcp` or `azure`, respectively. For example, `abc123.us-central1.gcp`. For details, see Snowflake's documentation: "[Specifying Region Information in Your Account Hostname](https://docs.snowflake.com/en/user-guide/intro-regions.html#specifying-region-information-in-your-account-hostname)".

Please also note that the Snowflake account name should only be the `account_name` without the prefixed `organization_name`. To determine if the region and/or cloud platform needs to be appended to the account locator in the legacy format, see Snowflake's documentation on "[Non-VPS account locator formats by cloud platform and region](https://docs.snowflake.com/en/user-guide/admin-account-identifier#non-vps-account-locator-formats-by-cloud-platform-and-region)".

### client\_session\_keep\_alive[​](#client_session_keep_alive "Direct link to client_session_keep_alive")

The `client_session_keep_alive` feature is intended to keep Snowflake sessions alive beyond the typical 4 hour timeout limit. The snowflake-connector-python implementation of this feature can prevent processes that use it (read: dbt) from exiting in specific scenarios. If you encounter this in your deployment of dbt, please let us know in [the GitHub issue](https://github.com/dbt-labs/dbt-core/issues/1271), and work around it by disabling the keepalive.

### platform\_detection\_timeout\_seconds[​](#platform_detection_timeout_seconds "Direct link to platform_detection_timeout_seconds")

The Snowflake connector uses the `platform_detection_timeout_seconds` parameter to determine how long it waits to detect the cloud platform for a connection. This parameter is available starting in dbt Core v1.10.

* Set to `0.0` (default) to disable cloud platform detection for faster connections.
* Set to a positive value only if you're using WIF authentication, which requires the connector to detect the cloud environment.

### query\_tag[​](#query_tag "Direct link to query_tag")

[Query tags](https://docs.snowflake.com/en/sql-reference/parameters.html#query-tag) are a Snowflake parameter that can be quite useful later on when searching in the [QUERY\_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/query_history.html).

### reuse\_connections[​](#reuse_connections "Direct link to reuse_connections")

During node execution (such as model and test), dbt opens connections against a Snowflake warehouse. Setting this configuration to `True` reduces execution time by verifying credentials only once for each thread.

### retry\_on\_database\_errors[​](#retry_on_database_errors "Direct link to retry_on_database_errors")

The `retry_on_database_errors` flag along with the `connect_retries` count specification is intended to make retries configurable after the snowflake connector encounters errors of type snowflake.connector.errors.DatabaseError. These retries can be helpful for handling errors of type "JWT token is invalid" when using key pair authentication.

By default, `retry_on_database_errors` is set to `False` when using dbt Core (for example, if you're running dbt locally with `pip install dbt-snowflake`).

However, in the dbt platform, this setting is automatically set to `True`, unless the user explicitly configures it.

### retry\_all[​](#retry_all "Direct link to retry_all")

The `retry_all` flag along with the `connect_retries` count specification is intended to make retries configurable after the snowflake connector encounters any error.
