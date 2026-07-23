# Connect Microsoft Fabric Lakehouse to dbt Core

`profiles.yml` file is for dbt Core and dbt fusion only

If you're using dbt platform, you don't need to create a `profiles.yml` file. This file is only necessary when you use dbt Core or dbt Fusion locally. To learn more about Fusion prerequisites, refer to [Supported features](https://docs.getdbt.com/docs/fusion/supported-features.md). To connect your data platform to dbt, refer to [About data platforms](https://docs.getdbt.com/docs/platform/connect-data-platform/about-connections.md).

Below is a guide for use with [Fabric Data Engineering](https://learn.microsoft.com/en-us/fabric/data-engineering/data-engineering-overview), a new product within Microsoft Fabric. This adapter currently supports connecting to a lakehouse endpoint.

To learn how to set up dbt using Fabric Warehouse, refer to [Microsoft Fabric Data Warehouse](https://docs.getdbt.com/docs/local/connect-data-platform/fabric-setup.md).

<!-- -->

* **Maintained by**:
  <!-- -->
  Microsoft
* **Authors**:
  <!-- -->
  Microsoft
* **GitHub repo**: [microsoft/dbt-fabricspark](https://github.com/microsoft/dbt-fabricspark) [![](https://img.shields.io/github/stars/microsoft/dbt-fabricspark?style=for-the-badge)](https://github.com/microsoft/dbt-fabricspark)
* **PyPI package**: `dbt-fabricspark` [![](https://badge.fury.io/py/dbt-fabricspark.svg)](https://badge.fury.io/py/dbt-fabricspark)
* **Slack channel**: [db-fabric-synapse](https://getdbt.slack.com/archives/C01DRQ178LQ)
* **Supported dbt Core version**:
  <!-- -->
  v1.7
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not supported
* **Minimum data platform version**:
  <!-- -->
  n/a

## Installing <!-- -->dbt-fabricspark

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-fabricspark`

## Configuring <!-- -->dbt-fabricspark<!-- -->

For <!-- -->Microsoft Fabric<!-- -->-specific configuration, please refer to [Microsoft Fabric<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/fabricspark-configs.md)

For further info, refer to the GitHub repository: [microsoft/dbt-fabricspark](https://github.com/microsoft/dbt-fabricspark)

## Supported authentication types[​](#supported-authentication-types "Direct link to Supported authentication types")

The Fabric Lakehouse adapter (`dbt-fabricspark`) connects to Fabric Spark through the Livy API. Choose the authentication method that matches where you run dbt Core.

* Azure CLI
* Service principal
* Fabric notebook
* Custom TokenCredential

Use Azure CLI for self-hosted dbt installations. This lets dbt Core use credentials from an active `az login` session. To use this method, set `authentication: CLI` in your `profiles.yml` file and run `az login`.

When you authenticate, Azure CLI may open a browser window or prompt you to complete sign-in on the [Microsoft device login](https://microsoft.com/devicelogin) page and enter a one-time code to complete sign-in. Once authentication is successful, dbt automatically reuses the active Azure CLI session for subsequent commands.

#### Example Azure CLI configuration[​](#example-azure-cli-configuration "Direct link to Example Azure CLI configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: fabricspark
      method: livy
      endpoint: https://api.fabric.microsoft.com/v1
      workspaceid: WORKSPACE_ID
      lakehouseid: LAKEHOUSE_ID
      lakehouse: LAKEHOUSE_NAME
      schema: LAKEHOUSE_OR_SCHEMA_NAME
      threads: 1

      # Authentication (CLI for self-hosted dbt installations)
      authentication: CLI
```

Use a Microsoft Entra ID service principal for CI/CD and other automated workloads. This method uses app registration credentials, not an active az login session. To use this method, set `authentication: SPN` in your `profiles.yml` file and provide the service principal's client ID, tenant ID, and client secret.

Before you run with service principal authentication, make sure:

* The service principal has access to the Fabric workspace and lakehouse.
* Service principals can use Fabric APIs is enabled in the Fabric Admin port.

#### Example service principal configuration[​](#example-service-principal-configuration "Direct link to Example service principal configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: fabricspark
      method: livy
      endpoint: https://api.fabric.microsoft.com/v1
      workspaceid: WORKSPACE_ID
      lakehouseid: LAKEHOUSE_ID
      lakehouse: LAKEHOUSE_NAME
      schema: LAKEHOUSE_OR_SCHEMA_NAME
      threads: 1

      # Authentication (SPN for CI/CD)
      authentication: SPN
      client_id: SPN_CLIENT_ID
      tenant_id: MICROSOFT_ENTRA_TENANT_ID
      client_secret: SPN_CLIENT_SECRET
```

You can authenticate through the Livy API from a Fabric notebook for production workloads and orchestration use cases within the Microsoft Fabric ecosystem. This method uses [`notebookutils.credentials`](https://learn.microsoft.com/en-us/fabric/data-engineering/notebookutils/notebookutils-credentials?tabs=python).

Use Python notebooks instead of PySpark notebooks to minimize compute costs because the Livy API starts its own Spark sessions.

#### Example Fabric notebook configuration[​](#example-fabric-notebook-configuration "Direct link to Example Fabric notebook configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: fabricspark
      method: livy
      endpoint: https://api.fabric.microsoft.com/v1
      workspaceid: WORKSPACE_ID
      lakehouseid: LAKEHOUSE_ID
      lakehouse: LAKEHOUSE_NAME
      schema: LAKEHOUSE_OR_SCHEMA_NAME
      threads: 1

      # Authentication (fabric_notebook for execution from a Fabric notebook)
      authentication: fabric_notebook
```

Use `token_credential` when you need dbt Core to load a custom [`azure.core.credentials.TokenCredential`](https://learn.microsoft.com/en-us/python/api/azure-core/azure.core.credentials.tokencredential) implementation, such as a workload identity federation or token broker flow. To use this method, set `authentication: token_credential` and provide the dotted path to your credential class.

#### Example custom TokenCredential configuration[​](#example-custom-tokencredential-configuration "Direct link to Example custom TokenCredential configuration")

profiles.yml

```yml
default:
  target: dev
  outputs:
    dev:
      type: fabricspark
      method: livy
      endpoint: https://api.fabric.microsoft.com/v1
      workspaceid: WORKSPACE_ID
      lakehouseid: LAKEHOUSE_ID
      lakehouse: LAKEHOUSE_NAME
      schema: LAKEHOUSE_OR_SCHEMA_NAME
      threads: 1

      # Authentication (custom TokenCredential)
      authentication: token_credential
      credential_class: my_package.auth.ExternalTokenCredential
      credential_kwargs:
        tenant_id: MICROSOFT_ENTRA_TENANT_ID
```

Replace `credential_class` and `credential_kwargs` with the class path and keyword arguments your custom credential implementation expects.

## Connection methods[​](#connection-methods "Direct link to Connection methods")

`dbt-fabricspark` can connect to Fabric Spark runtime using Fabric Livy API method. The Fabric Livy API allows submitting jobs in two different modes:

* [`session-jobs`](#session-jobs) A Livy session job entails establishing a Spark session that remains active throughout the Spark session. A Spark session, can run multiple jobs (each job is an action), sharing state and cached data between jobs.
* batch jobs entails submitting a Spark application for a single job execution. In contrast to a Livy session job, a batch job doesn't sustain an ongoing Spark session. With Livy batch jobs, each job initiates a new Spark session that ends when the job finishes.

Supported mode

To share the session state among jobs and reduce the overhead of session management, `dbt-fabricspark` adapter supports only `session-jobs` mode.

### session-jobs[​](#session-jobs "Direct link to session-jobs")

`session-jobs` is the preferred method when connecting to Fabric Lakehouse.

\~/.dbt/profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: fabricspark
      method: livy
      authentication: CLI
      endpoint: https://api.fabric.microsoft.com/v1
      workspaceid: [Fabric Workspace GUID]
      lakehouseid: [Lakehouse GUID]
      lakehouse: [Lakehouse Name]
      schema: [Lakehouse Name]
      spark_config:
        name: [Application Name]
        # optional
        archives:
          - "example-archive.zip"
        conf:
            spark.executor.memory: "2g"
            spark.executor.cores: "2"
        tags:
          project: [Project Name]
          user: [User Email]
          driverMemory: "2g"
          driverCores: 2
          executorMemory: "4g"
          executorCores: 4
          numExecutors: 3
      # optional
      connect_retries: 0
      connect_timeout: 10
      retry_all: true
```

## Optional configurations[​](#optional-configurations "Direct link to Optional configurations")

### Retries[​](#retries "Direct link to Retries")

Intermittent errors can crop up unexpectedly while running queries against Fabric Spark. If `retry_all` is enabled, `dbt-fabricspark` will naively retry any queries that fails, based on the configuration supplied by `connect_timeout` and `connect_retries`. It does not attempt to determine if the query failure was transient or likely to succeed on retry. This configuration is recommended in production environments, where queries ought to be succeeding. The default `connect_retries` configuration is 2.

For instance, this will instruct dbt to retry all failed queries up to 3 times, with a 5 second delay between each retry:

\~/.dbt/profiles.yml

```yaml
retry_all: true
connect_timeout: 5
connect_retries: 3
```

### Spark configuration[​](#spark-configuration "Direct link to Spark configuration")

Spark can be customized using [Application Properties](https://spark.apache.org/docs/latest/configuration.html). Using these properties the execution can be customized, for example, to allocate more memory to the driver process. Also, the Spark SQL runtime can be set through these properties. For example, this allows the user to [set a Spark catalogs](https://spark.apache.org/docs/latest/configuration.html#spark-sql).

### Supported functionality[​](#supported-functionality "Direct link to Supported functionality")

Most dbt Core functionality is supported, Please refer to [Delta Lake interoporability](https://learn.microsoft.com/en-us/fabric/fundamentals/delta-lake-interoperability).

Delta-only features:

1. Incremental model updates by `unique_key` instead of `partition_by` (see [`merge` strategy](https://docs.getdbt.com/reference/resource-configs/spark-configs.md#the-merge-strategy))
2. [Snapshots](https://docs.getdbt.com/docs/build/snapshots.md)
3. [Persisting](https://docs.getdbt.com/reference/resource-configs/persist_docs.md) column-level descriptions as database comments

### Limitations[​](#limitations "Direct link to Limitations")

1. Only Delta, CSV, and Parquet table data formats are supported by Fabric Lakehouse.
