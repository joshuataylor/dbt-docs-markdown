# Connect Snowflake Fusion compatible

dbt platformⓘ

<!-- -->

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

dbt platform connections and credentials inherit the permissions of the accounts configured. You can customize roles and associated permissions in Snowflake to fit your company's requirements and fine-tune access to database objects in your account.

Refer to [Snowflake permissions](https://docs.getdbt.com/reference/database-permissions/snowflake-permissions.md) for more information about customizing roles in Snowflake. To see which Snowflake functions are supported in Fusion in `strict` mode, refer to [Snowflake function support](https://docs.getdbt.com/reference/resource-configs/snowflake-function-support.md).

## Warehouse permissions for Fusion[​](#warehouse-permissions-for-fusion "Direct link to Warehouse permissions for Fusion")

<!-- -->

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

For role examples in Snowflake, see [Snowflake permissions](https://docs.getdbt.com/reference/database-permissions/snowflake-permissions.md).

## Connection fields[​](#connection-fields "Direct link to Connection fields")

The following fields are required when creating a Snowflake connection:

| Field     | Description                                                                                                                                                                                                             | Examples                                                                                                  |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| Account   | The Snowflake account to connect to. Take a look [here](https://docs.getdbt.com/docs/local/connect-data-platform/snowflake-setup.md#account) to determine what the account field should look like based on your region. |   ✅ `db5261993` or `db5261993.east-us-2.azure`<br />  ❌ `db5261993.eu-central-1.snowflakecomputing.com` |
| Role      | A mandatory field indicating what role should be assumed after connecting to Snowflake                                                                                                                                  | `transformer`                                                                                             |
| Database  | The logical database to connect to and run queries against.                                                                                                                                                             | `analytics`                                                                                               |
| Warehouse | The virtual warehouse to use for running queries.                                                                                                                                                                       | `transforming`                                                                                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Authentication methods[​](#authentication-methods "Direct link to Authentication methods")

This section describes the different authentication methods for connecting dbt to Snowflake. Configure deployment environment (Production, Staging, General) credentials globally in the [**Connections**](https://docs.getdbt.com/docs/deploy/deploy-environments.md#deployment-connection) area of **Account settings**. Individual users configure their user credentials in the [**Credentials**](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#get-started-with-the-studio-ide) area of their user profile.

Snowflake authentication in the dbt platform

You cannot create new Snowflake credentials with username and password in dbt platform. New development and deployment credentials default to [key pair](#key-pair) authentication. For user credentials on Enterprise-tier plans, [Snowflake OAuth](#snowflake-oauth) is also available when configured on the connection. To update existing password credentials, refer to [Username and password with MFA](#username-and-password-with-mfa).

### Key pair[​](#key-pair "Direct link to Key pair")

**Available in:** Development environments, Deployment environments

If you are creating Snowflake credentials for the first time in dbt platform, key pair is the default authentication method. Use it for both development and deployment credentials. The `Keypair` auth method uses Snowflake's [Key Pair Authentication](https://docs.snowflake.com/en/user-guide/key-pair-auth) to authenticate development or deployment credentials for a dbt project.

1. After [generating an encrypted key pair](https://docs.snowflake.com/en/user-guide/key-pair-auth.html#configuring-key-pair-authentication), be sure to set the `rsa_public_key` for the Snowflake user to authenticate in dbt:

   ```sql
   alter user jsmith set rsa_public_key='MIIBIjANBgkqh...';   
   ```

2. Finally, set the **Private Key** and **Private Key Passphrase** fields in the **Credentials** page to finish configuring dbt to authenticate with Snowflake using a key pair.

   * **Note:** Unencrypted private keys are permitted. Use a passphrase only if needed. dbt can specify a `private_key` directly as a string instead of a `private_key_path`. This `private_key` string can be in either Base64-encoded DER format, representing the key bytes, or in plain-text PEM format. Refer to [Snowflake documentation](https://docs.snowflake.com/en/user-guide/key-pair-auth) for more info on how they generate the key.
   * Specifying a private key using an [environment variable](https://docs.getdbt.com/docs/build/environment-variables.md) (for example, `{{ env_var('DBT_PRIVATE_KEY') }}`) is not supported.

3. To successfully fill in the Private Key field, you *must* include commented lines. If you receive a `Could not deserialize key data` or `JWT token` error, refer to [Troubleshooting](#troubleshooting) for more info.

**Example:**

```sql
-----BEGIN ENCRYPTED PRIVATE KEY-----
< encrypted private key contents here - line 1 >
< encrypted private key contents here - line 2 >
< ... >
-----END ENCRYPTED PRIVATE KEY-----
```

[![Snowflake keypair authentication](/img/docs/dbt-platform/snowflake-keypair-auth.png?v=2 "Snowflake keypair authentication")](#)Snowflake keypair authentication

#### Fusion key pair[​](#fusion-key-pair "Direct link to Fusion key pair")

<!-- -->

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

### Snowflake OAuth[​](#snowflake-oauth "Direct link to Snowflake OAuth")

**Available in:** Development environments, Enterprise-tier plans only

The OAuth auth method permits dbt to run development queries on behalf of a Snowflake user without the configuration of Snowflake password in dbt.

For more information on configuring a Snowflake OAuth connection in dbt, please see [the docs on setting up Snowflake OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-snowflake-oauth.md).

[![Configuring Snowflake OAuth connection](/img/docs/dbt-platform/dbt-platform-enterprise/database-connection-snowflake-oauth.png?v=2 "Configuring Snowflake OAuth connection")](#)Configuring Snowflake OAuth connection

Migrate from username and password

If your credentials still use username and password, you can view the existing configuration in **Credentials**, connection profiles, or deployment credential forms, but you cannot save changes until you switch **Auth method** to key pair or OAuth. A warning banner may also appear at the top of your account when password credentials are still in use.

To migrate off of username and password, follow the steps in [Key pair](#key-pair) or [Snowflake OAuth](#snowflake-oauth).

### Username and password with MFA[​](#username-and-password-with-mfa "Direct link to Username and password with MFA")

**Available in:** Existing development and deployment credentials only

If you are still on username and password while you plan your migration, [MFA](https://docs.snowflake.com/en/user-guide/security-mfa) is required by Snowflake for all password logins. Snowflake's MFA support is powered by the Duo Security service.

* In dbt, set the following [extended attribute](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) in the development environment **General settings** page, under the **Extended attributes** section:

  ```yaml
  authenticator: username_password_mfa
  ```

* To reduce the number of user prompts when connecting to Snowflake with MFA, [enable token caching](https://docs.snowflake.com/en/user-guide/security-mfa#using-mfa-token-caching-to-minimize-the-number-of-prompts-during-authentication-optional) in Snowflake.

* Optionally, if users miss prompts and their Snowflake accounts get locked, you can prevent automatic retries by adding the following in the same **Extended attributes** section:

  ```yaml
  connect_retries: 0
  ```

[![Configure the MFA username and password, and connect\_retries in the development environment settings.](/img/docs/dbt-platform/platform-configuring-dbt-platform/extended-attributes-mfa.png?v=2 "Configure the MFA username and password, and connect_retries in the development environment settings.")](#)Configure the MFA username and password, and connect\_retries in the development environment settings.

## Configuration[​](#configuration "Direct link to Configuration")

To learn how to optimize performance with data platform-specific configurations in dbt, refer to [Snowflake-specific configuration](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md).

### Custom domain URL[​](#custom-domain-url "Direct link to Custom domain URL")

To connect to Snowflake through a custom domain (vanity URL) instead of the account locator, use [extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) to configure the `host` parameter with the custom domain:

```yaml
host: https://custom_domain_to_snowflake.com
```

This configuration may conflict with Snowflake OAuth when used with PrivateLink. IF users can't reach Snowflake authentication servers from a networking standpoint, please [contact dbt Support](mailto:support@getdbt.com) to find a workaround with this architecture.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

If you're receiving a `Could not deserialize key data` or `JWT token` error, refer to the following causes and solutions:

 Error: \`Could not deserialize key data\`

Possible cause and solution for the error "Could not deserialize key data" in dbt.

* This could be because of mistakes like not copying correctly, missing dashes, or leaving out commented lines.

**Solution**:

* You can copy the key from its source and paste it into a text editor to verify it before using it in dbt.

 Error: \`JWT token\`

Possible cause and solution for the error "JWT token" in dbt.

* This could be a transient issue between Snowflake and dbt. When connecting to Snowflake, dbt gets a JWT token valid for only 60 seconds. If there's no response from Snowflake within this time, you might see a `JWT token is invalid` error in dbt.
* The public key was not entered correctly in Snowflake.

**Solutions**

* dbt needs to retry connections to Snowflake.
* Confirm and enter Snowflake's public key correctly. Additionally, you can reach out to Snowflake for help or refer to this Snowflake doc for more info: [Key-Based Authentication Failed with JWT token is invalid Error](https://community.snowflake.com/s/article/Key-Based-Authentication-Failed-with-JWT-token-is-invalid-Error).
