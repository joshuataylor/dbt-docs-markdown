# Python model configurations

Use `dbt.config()` to set configs for a Python model. This page lists the parameters needed. For how to write a Python model, refer to [Python models](../../docs/build/python-models.md).

Literal values only

`dbt.config()` only accepts literal values (strings, booleans, and numbers). For anything more complex, set the config in a [properties YAML file](../resource-properties/config.md). Refer to [Configuring Python models](../../docs/build/python-models.md#configuring-python-models).

## Common parameters

The following parameters work on every platform that supports Python models.

| Parameter                                                                            | Type           | Description                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`materialized`](./materialized.md) | `<string>`     | How dbt builds the model, such as `table` or `incremental`. Python models don't support `view`.                                                                                                                 |
| [`tags`](./tags.md)                 | `<list>`       | Tags to apply to the model.                                                                                                                                                                                     |
| [`meta`](./meta.md)                 | `<dictionary>` | Custom metadata. Read it with `dbt.config.meta_get()`.                                                                                                                                                          |
| `packages`                                                                           | `<list>`       | Packages to install, with optional version pins such as `numpy==1.23.1`. Required on some platforms. Refer to [Configuring packages](../../docs/build/python-models.md#configuring-packages). |

Read any config back inside your model with `dbt.config.get()`.

## Platform-specific parameters

### Snowflake

| Parameter                      | Type           | Default | Description                                                                                                                                                                                                          |
| ------------------------------ | -------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `python_version`               | `<string>`     | `3.10`  | Python version the Snowpark model runs on.                                                                                                                                                                           |
| `packages`                     | `<list>`       | `[]`    | Anaconda packages to install when the model runs. dbt always adds `snowflake-snowpark-python`.                                                                                                                       |
| `imports`                      | `<list>`       | `[]`    | Staged files, such as zip files, to make available to your model. Refer to [Third-party Snowflake packages](./snowflake-configs.md#third-party-snowflake-packages). |
| `external_access_integrations` | `<list>`       | `[]`    | Snowflake external access integrations that let your model reach external APIs.                                                                                                                                      |
| `secrets`                      | `<dictionary>` | `{}`    | Maps a variable name in your code to a Snowflake secret. Use it with `external_access_integrations`.                                                                                                                 |
| `use_anonymous_sproc`          | `<boolean>`    | `True`  | When `True`, dbt runs your model as an anonymous stored procedure. When `False`, dbt creates a named procedure (`<model_name>__dbt_sp`), calls it, then drops it.                                                    |

For more details and a more comprehensive list, refer to [Snowflake configurations](./snowflake-configs.md#additional-parameters).

### Databricks

| Parameter             | Type           | Default               | Description                                                                                             |
| --------------------- | -------------- | --------------------- | ------------------------------------------------------------------------------------------------------- |
| `submission_method`   | `<string>`     | `all_purpose_cluster` | How dbt submits the job: `all_purpose_cluster`, `job_cluster`, `serverless_cluster`, or `workflow_job`. |
| `create_notebook`     | `<boolean>`    | `false`               | With `all_purpose_cluster`, upload a notebook and run it as a job instead of using the Command API.     |
| `cluster_id`          | `<string>`     | `None`                | ID of an existing all-purpose cluster to run on.                                                        |
| `http_path`           | `<string>`     | `None`                | Path of an existing all-purpose cluster to run on.                                                      |
| `job_cluster_config`  | `<dictionary>` | `{}`                  | Settings for the new cluster used by `job_cluster` and `workflow_job`.                                  |
| `timeout`             | `<integer>`    | `0` (no timeout)      | Maximum time to wait for the command or job to run.                                                     |
| `packages`            | `<list>`       | `[]`                  | Packages to install on the cluster.                                                                     |
| `index_url`           | `<string>`     | `None` (uses PyPI)    | URL to install `packages` from.                                                                         |
| `additional_libs`     | `<list>`       | `[]`                  | Libraries to attach to the job.                                                                         |
| `access_control_list` | `<list>`       | none                  | Access control for the job.                                                                             |
| `python_job_config`   | `<dictionary>` | `{}`                  | Extra job and workflow settings.                                                                        |

Not every parameter works with every submission method. For more details and a more comprehensive list, refer to [Databricks configurations](./databricks-configs.md#python-submission-methods).

### BigQuery

| Parameter               | Type        | Default   | Description                                                  |
| ----------------------- | ----------- | --------- | ------------------------------------------------------------ |
| `submission_method`     | `<string>`  | none      | Where the job runs: `serverless`, `bigframes`, or `cluster`. |
| `dataproc_cluster_name` | `<string>`  | none      | Dataproc cluster to run on. Required for `cluster`.          |
| `dataproc_region`       | `<string>`  | none      | Google Cloud region where Dataproc is enabled.               |
| `gcs_bucket`            | `<string>`  | none      | Cloud Storage bucket for job artifacts.                      |
| `compute_region`        | `<string>`  | none      | Region of the job.                                           |
| `notebook_template_id`  | `<integer>` | none      | Colab Enterprise runtime template.                           |
| `packages`              | `<list>`    | none      | Packages to `pip install`. `bigframes` only.                 |
| `timeout`               | `<integer>` | none      | Maximum run time in seconds.                                 |
| `enable_list_inference` | `<boolean>` | `True`    | Let PySpark read multiple records in one operation.          |
| `intermediate_format`   | `<string>`  | `parquet` | File format for intermediate writes: `parquet` or `orc`.     |

For more details and a more comprehensive list, refer to [BigQuery configurations](./bigquery-configs.md#additional-parameters).
