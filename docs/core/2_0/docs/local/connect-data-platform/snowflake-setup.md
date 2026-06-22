# Connect Snowflake to Fusion [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Snowflake enforcing strong authentication

Starting August 31, 2026, password authentication will no longer be supported. Please update your environments to use key-pair or OAuth by that date to prevent service disruptions.

You can configure the Snowflake adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type. To check out which Snowflake functions are supported in Fusion in `strict` mode, refer to [Snowflake function support](https://docs.getdbt.com/reference/resource-configs/snowflake-function-support.md).

The Snowflake adapter for Fusion supports the following [authentication methods](#supported-authentication-types):

* Password
* Key pair
* Single sign-on (SSO)
* Password with MFA

note

[Snowflake is deprecating single-access password login](https://docs.snowflake.com/en/user-guide/security-mfa-rollout). Individual developers should use MFA or SSO instead of password authentication. Password-based login remains supported for service users (Snowflake user type: `LEGACY_SERVICE`).

## Warehouse permissions[​](#warehouse-permissions "Direct link to Warehouse permissions")

The Snowflake user or service account that dbt Fusion engine connects as must be able to run dbt workloads (queries, metadata, and typical materializations). Grant privileges through a Snowflake role assigned to that user.

### Required Snowflake objects[​](#required-snowflake-objects "Direct link to Required Snowflake objects")

Before connecting, these objects must exist:

| Object        | Purpose                                            |
| ------------- | -------------------------------------------------- |
| **Account**   | Your Snowflake account identifier                  |
| **User**      | Service account or user for Fusion                 |
| **Role**      | Role assigned to the user with required privileges |
| **Warehouse** | Virtual warehouse for compute                      |
| **Database**  | Target database or databases for dbt models        |
| **Schema**    | Target schema or schemas within the database       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Core operations[​](#core-operations "Direct link to Core operations")

The following are required permissions for fundamental dbt operations:

| Permission     | Object          | Purpose                                    |
| -------------- | --------------- | ------------------------------------------ |
| `USAGE`        | Warehouse       | Execute queries                            |
| `USAGE`        | Database        | Access the database                        |
| `USAGE`        | Schema          | Access schemas                             |
| `SELECT`       | Tables or views | Read existing data and sources             |
| `CREATE TABLE` | Schema          | Create models materialized as tables       |
| `CREATE VIEW`  | Schema          | Create models materialized as views        |
| `INSERT`       | Tables          | Load data into tables                      |
| `UPDATE`       | Tables          | Incremental model updates                  |
| `DELETE`       | Tables          | Incremental models using delete and insert |
| `TRUNCATE`     | Tables          | Full refresh of incremental models         |
| `DROP`         | Tables or views | Replace existing objects                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Metadata operations[​](#metadata-operations "Direct link to Metadata operations")

The following are required permissions for dbt metadata operations:

| Permission            | Object              | Purpose                         |
| --------------------- | ------------------- | ------------------------------- |
| `USAGE`               | INFORMATION\_SCHEMA | Query table and column metadata |
| `DESCRIBE TABLE`      | Tables              | Get schema information          |
| `SHOW OBJECTS`        | Schema              | List relations in schema        |
| `SHOW USER FUNCTIONS` | Schema              | Discover UDFs (if used)         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Schema and database management[​](#schema-and-database-management "Direct link to Schema and database management")

The following are conditional permissions for schema and database management:

| Permission        | Object   | When required                       |
| ----------------- | -------- | ----------------------------------- |
| `CREATE SCHEMA`   | Database | Fusion should auto-create schemas   |
| `CREATE DATABASE` | Account  | Fusion should auto-create databases |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Advanced features[​](#advanced-features "Direct link to Advanced features")

The following are optional permissions for advanced features:

| Permission           | Object | Feature                             |
| -------------------- | ------ | ----------------------------------- |
| `CREATE STAGE`       | Schema | File staging for seeds or snapshots |
| `CREATE FILE FORMAT` | Schema | Custom file formats                 |
| `CREATE SEQUENCE`    | Schema | Sequences                           |
| `EXECUTE TASK`       | Schema | Snowflake tasks                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For role examples and SQL grants in Snowflake, you can also refer to [Snowflake permissions](https://docs.getdbt.com/reference/database-permissions/snowflake-permissions.md).

## Snowflake configuration details[​](#snowflake-configuration-details "Direct link to Snowflake configuration details")

The information required for configuring the Snowflake adapter can be found conveniently in your Snowflake account menu:

1. Click on your name from the Snowflake sidebar.
2. Hover over the **Account** field.
3. In the field with your account name, click **View account details**.
4. Click **Config file** and select the appropriate **Warehouse** and **Database**.

[![Sample config file in Snowflake.](/img/fusion/connect-adapters/snowflake-account-details.png?v=2 "Sample config file in Snowflake.")](#)Sample config file in Snowflake.

## Configure Fusion[​](#configure-fusion "Direct link to Configure Fusion")

Executing `dbt init` in your CLI will prompt for the following fields:

* **Account:** Snowflake account number
* **User:** Your Snowflake username
* **Database:** The database within your Snowflake account to connect to your project
* **Warehouse:** The compute warehouse that will handle the tasks for your project
* **Schema:** The development/staging/deployment schema for the project
* **Role (Optional):** The role dbt should assume when connnecting to the warehouse

Alternatively, you can manually create the `profiles.yml` file and configure the fields. See examples in [authentication](#supported-authentication-types) section for formatting. If there is an existing `profiles.yml` file, you are given the option to retain the existing fields or overwrite them.

Next, select your authentication method. Follow the on-screen prompts to provide the required information.

## Supported authentication types[​](#supported-authentication-types "Direct link to Supported authentication types")

* Password
* Key pair
* Single sign-on

Password authentication prompts for your Snowflake account password. This is becoming an increasingly less common option as organizations opt for more secure authentication.

Selecting **Password with MFA** redirects you to the Snowflake account login to provide your passkey or authenticator password.

#### Example password configuration[​](#example-password-configuration "Direct link to Example password configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: snowflake
      threads: 16
      account: ABC123
      user: JANE.SMITH@YOURCOMPANY.COM
      database: JAFFLE_SHOP
      warehouse: TRANSFORM
      schema: JANE_SMITH
      password: THISISMYPASSWORD
```

#### Example password with MFA configuration[​](#example-password-with-mfa-configuration "Direct link to Example password with MFA configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: snowflake
      threads: 16
      authenticator: username_password_mfa
      account: ABC123
      user: JANE.SMITH@YOURCOMPANY.COM
      database: JAFFLE_SHOP
      warehouse: TRANFORM
      schema: JANE_SMITH
```

Key pair authentication gives you the option to:

* Define the path to the key.
* Provide the plain-text PEM format key inline.

We recommend using PKCS#8 format with AES-256 encryption for key pair authentication with Fusion. Fusion doesn't support legacy 3DES encryption or headerless key formats. Using older key formats may cause authentication failures.

If you encounter the `Key is PKCS#1 (RSA private key). Snowflake requires PKCS#8` error, then your private key is in the wrong format. You have two options:

* (Recommended fix) Re-export your key with modern encryption:

  ```bash
  # Convert to PKCS#8 with AES-256 encryption
  openssl genrsa 2048 | openssl pkcs8 -topk8 -v2 aes-256-cbc -inform PEM -out rsa_key.p8
  ```

* (Temporary workaround) Add the `BEGIN` header and `END` footer to your PEM body:

  ```text
  -----BEGIN ENCRYPTED PRIVATE KEY-----
  < Your existing encrypted private key contents >
  -----END ENCRYPTED PRIVATE KEY-----
  ```

Once the key is configuted, you will be given the option to provide a passphrase, if required.

#### Example key pair configuration[​](#example-key-pair-configuration "Direct link to Example key pair configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: snowflake
      threads: 16
      account: ABC123
      user: JANE.SMITH@YOURCOMPANY.COM
      database: JAFFLE_SHOP
      warehouse: TRANSFORM
      schema: JANE_SMITH
      private_key: '<Your existing encrypted private key contents>'
      private_key_passphrase: YOURPASSPHRASEHERE
```

Single sign-on will leverage your browser to authenticate the Snowflake session.

By default, every connection that dbt opens will require you to re-authenticate in a browser. The Snowflake connector package supports caching your session token, but it [currently only supports Windows and Mac OS](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-use.html#optional-using-connection-caching-to-minimize-the-number-of-prompts-for-authentication).

Refer to the [Snowflake docs](https://docs.snowflake.com/en/sql-reference/parameters.html#label-allow-id-token) for information on enabling this feature in your account.

#### Example SSO configuration[​](#example-sso-configuration "Direct link to Example SSO configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: snowflake
      threads: 16
      authenticator: externalbrowser
      account: ABC123
      user: JANE.SMITH@YOURCOMPANY.COM
      database: JAFFLE_SHOP
      warehouse: TRANSFORM
      schema: JANE_SMITH
```

## More information[​](#more-information "Direct link to More information")

Find Snowflake-specific configuration information in the [Snowflake adapter reference guide](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md).
