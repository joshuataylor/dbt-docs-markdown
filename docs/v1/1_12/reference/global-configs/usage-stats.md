# Anonymous usage stats

dbt Labs is on a mission to build the best version of dbt possible, and a crucial part of that is understanding how users work with dbt. To this end, we've added some simple event tracking (or telemetry) to dbt using Snowplow. Importantly, we do not track credentials, raw model contents, or model names: we consider these private, and frankly none of our business.

The data we collect is used for use cases such as industry identification, use-case research, improvements of sales, marketing, product features, and services. Telemetry allows users to seamlessly contribute to the continuous improvement of dbt, enabling us to better serve the data community.

Usage statistics are fired when dbt is invoked and when models are run. These events contain basic platform information (OS + Python version) and metadata such as:

* Whether the invocation succeeded.
* How long it took.
* An anonymized hash key representing the raw model content.
* Number of nodes that were run.

## dbt Core telemetry[​](#dbt-core-telemetry "Direct link to dbt Core telemetry")

dbt Core has telemetry enabled by default. For full transparency, you can see all the event definitions in [`tracking.py`](https://github.com/dbt-labs/dbt-core/blob/1.latest/core/dbt/tracking.py).

You can opt out of event tracking at any time by adding the following configuration to your `dbt_project.yml` file:

dbt\_project.yml

```yaml
flags:
  send_anonymous_usage_stats: false
```

You can also set the `DO_NOT_TRACK` environment variable. For more information, refer to [Environment variables](https://docs.getdbt.com/docs/build/environment-variables.md).

`DO_NOT_TRACK=1` is equivalent to `DBT_ENGINE_SEND_ANONYMOUS_USAGE_STATS=False`.
