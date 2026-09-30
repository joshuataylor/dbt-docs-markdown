(Applies to dbt v2.0 and later)

# Connect Redshift to dbt v2

Local development

You can configure the Redshift adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type.

The Redshift adapter for dbt v2 supports the following [authentication methods](#supported-authentication-types):

* Password
* IAM profile
* IAM Identity Center (browser)

## Warehouse permissions

The Redshift database user that dbt v2 uses must be able to run dbt workloads and read catalog metadata used for introspection.

### Required Redshift objects

Before connecting, these objects must exist or be accessible:

| Object                                                  | Purpose                           |
| ------------------------------------------------------- | --------------------------------- |
| **Cluster** (provisioned) or **workgroup** (serverless) | Compute resource                  |
| **Database**                                            | Target database                   |
| **Schema**                                              | Target schema within the database |
| **User**                                                | Database user for authentication  |
| **IAM role or profile** (optional)                      | For IAM-based authentication      |

### Core permissions

The following permissions are required for fundamental dbt features:

| Permission | Object          | Purpose                               |
| ---------- | --------------- | ------------------------------------- |
| `USAGE`    | Schema          | Access the schema                     |
| `CREATE`   | Schema          | Create tables and views in the schema |
| `SELECT`   | Tables or views | Read data                             |
| `INSERT`   | Tables          | Insert data                           |
| `UPDATE`   | Tables          | Update data                           |
| `DELETE`   | Tables          | Delete data                           |
| `DROP`     | Tables or views | Drop or replace objects               |
| `TRUNCATE` | Tables          | Truncate tables                       |

### Metadata operations

dbt v2 queries these Redshift system relations:

| System relation    | Purpose                                                | Permission required       |
| ------------------ | ------------------------------------------------------ | ------------------------- |
| `SVV_ALL_COLUMNS`  | Column metadata                                        | SELECT on the system view |
| `pg_class`         | List relations                                         | Access to system catalog  |
| `pg_namespace`     | Schema information                                     | Access to system catalog  |
| `sys_query_detail` | Source freshness (last insert time)                    | SELECT on the system view |
| `svv_table_info`   | List materialized views when the project includes them | SELECT on the system view |
| `svv_mv_info`      | List materialized views when the project includes them | SELECT on the system view |

### Schema management

Conditional permissions for schema management

| Permission      | Object   | When required       |
| --------------- | -------- | ------------------- |
| `CREATE SCHEMA` | Database | Auto-create schemas |

For example SQL grants in Redshift, refer to [Redshift permissions](../../../reference/database-permissions/redshift-permissions.md).

## Configure dbt v2

Executing `dbt init` in your CLI will prompt for the following fields:

* **Host:** The hostname of your Redshift cluster
* **User:** Username of the account that will be connecting to the database
* **Database:** The database name
* **Schema:** The schema name
* **Port (default: 5439):** Port for your Redshift environment

Alternatively, you can manually create the `profiles.yml` file and configure the fields. See examples in [authentication](#supported-authentication-types) section for formatting. If there is an existing `profiles.yml` file, you are given the option to retain the existing fields or overwrite them.

Next, select your authentication method. Follow the on-screen prompts to provide the required information.

## Supported authentication types

### Password

Use your Redshift user's password to authenticate. You can also manually enter it in plain text into the `profiles.yml` file configuration.

#### Example password configuration

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: redshift
      port: 5439
      database: JAFFLE_SHOP
      schema: JAFFLE_TEST
      ra3_node: true
      method: database
      host: ABC123.COM
      user: JANE.SMITH@YOURCOMPANY.COM
      password: ABC123
      threads: 16
```

Report incorrect code

### IAM profile

Specify the IAM profile to use to connect your v2 sessions. You will need to provide the following information:

* **IAM Profile:** The profile name
* **Cluster ID:** The unique identifier for your AWS cluster
* **Region:** Your AWS region (for example, us-east-1)
* **Use RA3 node type (y/n):** Use high performance AWS RA3 node

#### Example IAM profile configuration

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: redshift
      port: 5439
      database: JAFFLE_SHOP
      schema: JAFFLE_TEST
      ra3_node: false
      method: iam
      host: YOURHOSTNAME.COM
      user: JANE.SMITH@YOURCOMPANY.COM
      iam_profile: YOUR_PROFILE_NAME
      cluster_id: ABC123
      region: us-east-1
      threads: 16
```

Report incorrect code

### IAM Identity Center (browser)

Sign in through [AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) and set `method` to `browser_identity_center`. dbt opens a browser window for you to authenticate without needing a password or IAM profile in your `profiles.yml`.

Local only

This method works when you run dbt locally from the command line. It isn't supported in the dbt platform yet.

Before you start, make sure your Redshift cluster or workgroup is set up for [IAM Identity Center integration](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-idp-connect.html). You need the following fields:

| Profile field             | Required | Default                  | Description                                                                                                                    |
| ------------------------- | -------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------ |
| `method`                  | Yes      | —                        | Set to `browser_identity_center`.                                                                                              |
| `idc_region`\*            | Yes      | —                        | AWS region where your IAM Identity Center instance lives (for example, `us-east-1`).                                           |
| `issuer_url`              | Yes      | —                        | Issuer URL of your IAM Identity Center instance (for example, `https://identitycenter.amazonaws.com/ssoins-1234567890abcdef`). |
| `idp_listen_port`         | No       | `7890`                   | Local port dbt listens on for the browser redirect after you sign in.                                                          |
| `idc_client_display_name` | No       | `Amazon Redshift driver` | App name shown in the browser consent prompt.                                                                                  |
| `idp_response_timeout`    | No       | `60`                     | Seconds dbt waits for you to finish signing in before timing out.                                                              |

\*`idc_region` isn't the same as your AWS account region. dbt doesn't support setting `region` alongside `browser_identity_center` in `profiles.yml` yet, so it reads your AWS region from your AWS config file (`~/.aws/config`). Make sure a default region is set there (for example, by running `aws configure`).

#### Example IAM Identity Center configuration

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: redshift
      method: browser_identity_center
      host: hostname.region.redshift.amazonaws.com
      port: 5439
      dbname: analytics
      schema: analytics
      idc_region: us-east-1
      issuer_url: https://identitycenter.amazonaws.com/ssoins-1234567890abcdef

      # Optional
      idp_listen_port: 7890
      idc_client_display_name: Amazon Redshift driver
      idp_response_timeout: 60
      threads: 4
```

Report incorrect code

## More information

Find Redshift-specific configuration information in the [Redshift adapter reference guide](../../../reference/resource-configs/redshift-configs.md).
