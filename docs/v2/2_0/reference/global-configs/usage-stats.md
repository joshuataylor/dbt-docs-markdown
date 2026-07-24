# Anonymous usage stats

dbt Labs is on a mission to build the best version of dbt possible, and a crucial part of that is understanding how users work with dbt. To this end, we've added some simple event tracking (or telemetry) to dbt using Snowplow. Importantly, we do not track credentials, raw model contents, or model names: we consider these private, and frankly none of our business.

The data we collect is used for use cases such as industry identification, use-case research, improvements of sales, marketing, product features, and services. Telemetry allows users to seamlessly contribute to the continuous improvement of dbt, enabling us to better serve the data community.

Usage statistics are fired when dbt is invoked and when models are run. These events contain basic platform information (OS + Python version) and metadata such as:

* Whether the invocation succeeded.
* How long it took.
* An anonymized hash key representing the raw model content.
* Number of nodes that were run.

<!-- -->

## dbt Fusion engine telemetry[​](#dbt-fusion-engine-telemetry "Direct link to dbt Fusion engine telemetry")

Fusion has telemetry enabled by default. For full transparency, you can see the event definitions in [`event_functions.rs`](https://github.com/dbt-labs/dbt-core/blob/main/crates/vortex-events/src/event_functions.rs).

Telemetry requires outbound HTTPS access to `https://p.vx.dbt.com`. If the endpoint is unreachable, Fusion logs errors on each invocation. For the complete list of outbound endpoints, refer to [Fusion networking requirements](https://docs.getdbt.com/docs/local/fusion-networking-requirements.md).

To disable anonymous usage statistics, set the following environment variable:

```shell
export DBT_ENGINE_SEND_ANONYMOUS_USAGE_STATS=false
```

You can also set `DO_NOT_TRACK=1`.
