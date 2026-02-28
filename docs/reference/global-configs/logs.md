# Logs

### Log formatting[​](#log-formatting "Direct link to Log formatting")

dbt outputs logs to two different locations: CLI console and the log file.

The `LOG_FORMAT` and `LOG_FORMAT_FILE` configs specify how dbt's logs should be formatted, and they each have the same options: `json`, `text`, and `debug`.

Usage

```text
dbt run --log-format json
```

The `text` format is the default for console logs and has plain text messages prefixed with a simple timestamp:

```text
23:30:16  Running with dbt=1.8.0
23:30:17  Registered adapter: postgres=1.8.0
```

The `debug` format is the default for the log file and is the same as the `text` format but with a more detailed timestamp and also includes the [`invocation_id`](https://docs.getdbt.com/reference/dbt-jinja-functions/invocation_id.md), [`thread_id`](https://docs.getdbt.com/reference/dbt-jinja-functions/thread_id.md), and [log level](https://docs.getdbt.com/reference/global-configs/logs.md#log-level) of each message:

```text
============================== 16:12:08.555032 | 9089bafa-4010-4f38-9b42-564ec9106e07 ==============================
16:12:08.555032 [info ] [MainThread]: Running with dbt=1.8.0
16:12:08.751069 [info ] [MainThread]: Registered adapter: postgres=1.8.0
```

The `json` format outputs fully structured logs in the JSON format:

```json
{"data": {"log_version": 3, "version": "=1.8.0"}, "info": {"category": "", "code": "A001", "extra": {}, "invocation_id": "82131fa0-d2b4-4a77-9436-019834e22746", "level": "info", "msg": "Running with dbt=1.8.0", "name": "MainReportVersion", "pid": 7875, "thread": "MainThread", "ts": "2024-05-29T23:32:54.993336Z"}}
{"data": {"adapter_name": "postgres", "adapter_version": "=1.8.0"}, "info": {"category": "", "code": "E034", "extra": {}, "invocation_id": "82131fa0-d2b4-4a77-9436-019834e22746", "level": "info", "msg": "Registered adapter: postgres=1.8.0", "name": "AdapterRegistered", "pid": 7875, "thread": "MainThread", "ts": "2024-05-29T23:32:56.437986Z"}}
```

When the `LOG_FORMAT` is set explicitly, it will take effect in both the console and log files, whereas the `LOG_FORMAT_FILE` only affects the log file.

Usage

```text
dbt run --log-format-file json
```

Tip: verbose structured logs

Use `json` formatting value in conjunction with the `DEBUG` config to produce rich log information which can be piped into monitoring tools for analysis:

```text
dbt run --debug --log-format json
```

See [structured logging](https://docs.getdbt.com/reference/events-logging.md#structured-logging) for more details.

### Log Level[​](#log-level "Direct link to Log Level")

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

### Debug-level logging[​](#debug-level-logging "Direct link to Debug-level logging")

The `DEBUG` config redirects dbt's debug logs to standard output. This has the effect of showing debug-level log information in the terminal in addition to the `logs/dbt.log` file. This output is verbose.

The `--debug` flag is also available via shorthand as `-d`.

Usage

```text
dbt run --debug
```

### Log and target paths[​](#log-and-target-paths "Direct link to Log and target paths")

By default, dbt will write logs to a directory named `logs/`, and all other artifacts to a directory named `target/`. Both of those directories are located relative to `dbt_project.yml` of the active project.

Just like other global configs, it is possible to override these values for your environment or invocation by using CLI options (`--target-path`, `--log-path`) or environment variables (`DBT_TARGET_PATH`, `DBT_LOG_PATH`).

### Suppress non-error logs in output[​](#suppress-non-error-logs-in-output "Direct link to Suppress non-error logs in output")

By default, dbt shows all logs in standard out (stdout). You can use the `QUIET` config to show only error logs in stdout. Logs will still include the output of anything passed to the [`print()`](https://docs.getdbt.com/reference/dbt-jinja-functions/print.md) macro. For example, you might suppress all but error logs to more easily find and debug a Jinja error.

profiles.yml

```yaml
config:
  quiet: true
```

Supply the `-q` or `--quiet` flag to `dbt run` to show only error logs and suppress non-error logs.

```text
dbt run --quiet
```

### dbt list logging[​](#dbt-list-logging "Direct link to dbt list logging")

In [dbt version 1.5](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.5.md#behavior-changes>), we updated the logging behavior of the [dbt list](https://docs.getdbt.com/reference/commands/list.md) command to include `INFO` level logs by default.

You can use either of these parameters to ensure clean output that's compatible with downstream processes, such as piping results to [`jq`](https://jqlang.github.io/jq/manual/), a file, or another process:

* `dbt list --log-level warn` (recommended; equivalent to previous default)
* `dbt list --quiet` (suppresses all logging less than `ERROR` level, except for "printed" messages and list output)

### Logging relational cache events[​](#logging-relational-cache-events "Direct link to Logging relational cache events")

<!-- -->

The `LOG_CACHE_EVENTS` config allows detailed logging for [relational cache](https://docs.getdbt.com/reference/global-configs/cache), which are disabled by default.

```text
dbt compile --log-cache-events
```

### Color[​](#color "Direct link to Color")

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
