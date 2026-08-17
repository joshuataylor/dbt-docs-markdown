(Applies to dbt v2.0 and later)

# Connect Redshift to Fusion [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

You can configure the Redshift adapter by running `dbt init` in your CLI or manually providing the `profiles.yml` file with the fields configured for your authentication type.

The Redshift adapter for Fusion supports the following [authentication methods](#supported-authentication-types):

* Password
* IAM profile

## Warehouse permissions

The Redshift database user that dbt Fusion engine uses must be able to run dbt workloads and read catalog metadata used for introspection.

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

Fusion queries these Redshift system relations:

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

## Configure Fusion

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

### IAM profile

Specify the IAM profile to use to connect your Fusion sessions. You will need to provide the following information:

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

## More information

Find Redshift-specific configuration information in the [Redshift adapter reference guide](../../../reference/resource-configs/redshift-configs.md).
