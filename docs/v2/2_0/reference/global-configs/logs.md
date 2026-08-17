# Logs

### Log formatting

dbt outputs logs to two different locations: CLI console and the log file.

(Applies to dbt v2.0 and later)

The `LOG_FORMAT` and `LOG_FORMAT_FILE` configs specify how dbt's logs should be formatted. Shared options are `text`, `json`, and `default`. The `otel` option is available for `LOG_FORMAT` only (console output). Setting `--log-format-file otel` has no effect.

The `otel` format streams [OpenTelemetry](https://opentelemetry.io/)-style structured telemetry to the console. It uses a different schema than dbt Core's `json` logs. For JSONL files, Parquet export, OTLP, and how this maps to dbt Core structured logging, refer to [Fusion telemetry and observability](../telemetry-observability.md).

Usage

```text
dbt build --log-format otel
```

(Applies to dbt v2.0 and later)

The `text` format is the default for console logs and prints plain text progress messages:

```text
dbt-fusion 2.0.0-preview.181
   Loading ~/.dbt/profiles.yml
```

The `json` format outputs fully structured logs in the JSON format:

(Applies to dbt v2.0 and later)

```json
{"data": {"log_version": 3, "version": "=2.0.0-preview.181"}, "info": {"category": "", "code": "A001", "extra": {}, "invocation_id": "019fb7d4-ce89-7712-8d06-5ad013a23be9", "level": "info", "msg": "Running with dbt-fusion=2.0.0-preview.181", "name": "MainReportVersion", "pid": 92554, "thread": "tokio-rt-worker", "ts": "2026-07-31T11:00:04.877509Z"}}
{"data": {"msg": "Loading ~/.dbt/profiles.yml"}, "info": {"category": "", "code": "", "extra": {}, "invocation_id": "019fb7d4-ce89-7712-8d06-5ad013a23be9", "level": "info", "msg": "Loading ~/.dbt/profiles.yml", "name": "Generic", "pid": 92554, "thread": "tokio-rt-worker", "ts": "2026-07-31T11:00:04.882928Z"}}
```

(Applies to dbt v2.0 and later)

When the `LOG_FORMAT` is set explicitly to `text`, `json`, or `default`, it takes effect in both the console and log files. The `otel` value applies to console output only. Use `LOG_FORMAT_FILE` to set a different format for the log file (`text`, `json`, or `default`).

Usage

```text
dbt run --log-format-file json
```

(Applies to dbt v2.0 and later)

Tip: structured observability

Use `--log-format otel` to stream OpenTelemetry-style telemetry to the console, or use `--otel-file-name` and related flags for file and platform integrations. Refer to [Fusion telemetry and observability](../telemetry-observability.md).

For JSON-formatted log lines, use `--log-format json` with the `DEBUG` config:

```text
dbt build --debug --log-format json
```

### Log Level

The `LOG_LEVEL` config sets the minimum severity of events captured in the console and file logs. This is a more flexible alternative to the `--debug` flag. The available options for the log levels are `debug`, `info`, `warn`, `error`, or `none`.

* Setting the `--log-level` will configure console and file logs.

  ```text
  dbt run --log-level debug
  ```

* Setting the `LOG_LEVEL` to `none` will disable information from being sent to either the console or file logs.

  ```text
  dbt run --log-level none
  ```

* To set the file log level as a different value than the console, use the `--log-level-file` flag.

  ```text
  dbt run --log-level-file error
  ```

* To only disable writing to the logs file but keep console logs, set `LOG_LEVEL_FILE` config to none.

  ```text
  dbt run --log-level-file none
  ```

### Debug-level logging

The `DEBUG` config redirects dbt's debug logs to standard output. This has the effect of showing debug-level log information in the terminal in addition to the `logs/dbt.log` file. This output is verbose.

The `--debug` flag is also available via shorthand as `-d`.

Usage

```text
dbt run --debug
```

### Log and target paths

By default, dbt will write logs to a directory named `logs/`, and all other artifacts to a directory named `target/`. Both of those directories are located relative to `dbt_project.yml` of the active project.

Just like other global configs, it is possible to override these values for your environment or invocation by using CLI options (`--target-path`, `--log-path`) or environment variables ((Applies to dbt v1.11 and later) `DBT_ENGINE_TARGET_PATH`, `DBT_ENGINE_LOG_PATH`).

### Suppress non-error logs in output

By default, dbt shows all logs in standard out (stdout). You can use the `QUIET` config to show only error logs in stdout. Logs will still include the output of anything passed to the [`print()`](../dbt-jinja-functions/print.md) macro. For example, you might suppress all but error logs to more easily find and debug a Jinja error.

profiles.yml

```yaml
config:
  quiet: true
```

Supply the `-q` or `--quiet` flag to `dbt run` to show only error logs and suppress non-error logs.

```text
dbt run --quiet
```

### dbt list logging

In [dbt version 1.5](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.5.md#behavior-changes>), we updated the logging behavior of the [dbt list](../commands/list.md) command to include `INFO` level logs by default.

You can use either of these parameters to ensure clean output that's compatible with downstream processes, such as piping results to [`jq`](https://jqlang.github.io/jq/manual/), a file, or another process:

* `dbt list --log-level warn` (recommended; equivalent to previous default)
* `dbt list --quiet` (suppresses all logging less than `ERROR` level, except for "printed" messages and list output)

### Logging relational cache events

The `LOG_CACHE_EVENTS` config allows detailed logging for [relational cache](https://docs.getdbt.com/reference/global-configs/cache), which are disabled by default.

```text
dbt compile --log-cache-events
```

### Color

You can set the color preferences for the file logs only within `profiles.yml` or using the `--use-colors-file / --no-use-colors-file` flags.

profiles.yml

```yaml
config:
  use_colors_file: False
```

```text
dbt run --use-colors-file
dbt run --no-use-colors-file
```
