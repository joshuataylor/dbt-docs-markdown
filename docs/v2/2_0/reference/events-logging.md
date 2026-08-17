# Events and logs

As dbt runs, it generates events. The most common way to see those events is as log messages, written in real time to two places:

* The command line terminal (`stdout`), to provide interactive feedback while running dbt.
* The debug log file (`logs/dbt.log`), to enable detailed [debugging of errors](../guides/debug-errors.md) when they occur. The text-formatted log messages in this file include all `DEBUG`-level events, as well as contextual information, such as log level and thread name. The location of this file can be configured via [the `log-path` flag](./global-configs/logs.md).

CLI

```bash
21:35:48  6 of 7 OK created view model dbt_testing.name_list......................... [CREATE VIEW in 0.17s]
```

logs/dbt.log

```text
============================== 21:21:15.272780 | 48cef052-3819-4550-a83a-4a648aef5a31 ==============================
21:21:15.272780 [info ] [MainThread]: Running with dbt=1.5.0-b5
21:21:15.273802 [debug] [MainThread]: running dbt with arguments {'printer_width': '80', 'indirect_selection': 'eager', 'log_cache_events': 'False', 'write_json': 'True', 'partial_parse': 'True', 'cache_selected_only': 'False', 'warn_error': 'None', 'fail_fast': 'False', 'debug': 'False', 'log_path': '/Users/jerco/dev/scratch/testy/logs', 'profiles_dir': '/Users/jerco/.dbt', 'version_check': 'False', 'use_colors': 'False', 'use_experimental_parser': 'False', 'no_print': 'None', 'quiet': 'False', 'log_format': 'default', 'static_parser': 'True', 'introspect': 'True', 'warn_error_options': 'WarnErrorOptions(include=[], exclude=[])', 'target_path': 'None', 'send_anonymous_usage_stats': 'True'}
21:21:16.190990 [debug] [MainThread]: Partial parsing enabled: 0 files deleted, 0 files added, 0 files changed.
21:21:16.191404 [debug] [MainThread]: Partial parsing enabled, no changes found, skipping parsing
21:21:16.207330 [info ] [MainThread]: Found 2 models, 0 tests, 0 snapshots, 1 analysis, 535 macros, 0 operations, 1 seed file, 0 sources, 0 exposures, 0 metrics, 0 groups
```

## Structured logging

(Applies to dbt v2.0 and later)

The dbt Fusion engine emits structured run telemetry using [OpenTelemetry](https://opentelemetry.io/) conventions instead of dbt Core's JSON event logs. To view it locally, use `--log-format otel` or the options in [Fusion telemetry and observability](./telemetry-observability.md#available-output-formats).

In dbt platform, you can [download OTel logs](../docs/deploy/run-visibility.md#access-logs) from Fusion job runs.

For `--log-format`, `--log-level`, and related CLI configs, refer to [Logs](./global-configs/logs.md).

## Python interface

(Applies to dbt v2.0 and later)

The dbt Fusion engine doesn't share dbt Core's Python event interface.

We are currently [developing](https://github.com/dbt-labs/dbt-core/issues/13102) a Python API with Rust bindings for programmatic invocations.
