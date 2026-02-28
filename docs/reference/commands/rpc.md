# About dbt rpc command

The dbt-rpc plugin is deprecated

dbt Labs actively maintained `dbt-rpc` for compatibility with dbt-core versions up to v1.5. Starting with dbt-core v1.6 (released in July 2023), `dbt-rpc` is no longer supported for ongoing compatibility.

In the meantime, dbt Labs will be performing critical maintenance only for `dbt-rpc`, until the last compatible version of dbt-core has reached the [end of official support](https://docs.getdbt.com/docs/dbt-versions/core.md#latest-releases). At that point, dbt Labs will archive this repository to be read-only.

### Overview[​](#overview "Direct link to Overview")

You can use the `dbt-rpc` plugin to run a Remote Procedure Call (rpc) dbt server. This server compiles and runs queries in the context of a dbt project. Additionally, the RPC server provides methods that enable you to list and terminate running processes. We recommend running an rpc server from a directory containing a dbt project. The server will compile the project into memory, then accept requests to operate against that project's dbt context.

Running on Windows

We do not recommend running the rpc server on Windows because of reliability issues. A Docker container may provide a useful workaround, if required.

For more details, see the [`dbt-rpc` repository](https://github.com/dbt-labs/dbt-rpc) source code.

**Running the server:**

```text

$ dbt-rpc serve
Running with dbt=1.5.0

16:34:31 | Concurrency: 8 threads (target='dev')
16:34:31 |
16:34:31 | Done.
Serving RPC server at 0.0.0.0:8580
Send requests to http://localhost:8580/jsonrpc
```

**Configuring the server**

* `--host`: Specify the host to listen on (default=`0.0.0.0`)
* `--port`: Specify the port to listen on (default=`8580`)

**Submitting queries to the server:** The rpc server expects requests in the following format:

rpc-spec.json

```json
{
    "jsonrpc": "2.0",
    "method": "{ a valid rpc server command }",
    "id": "{ a unique identifier for this query }",
    "params": {
        "timeout": { timeout for the query in seconds, optional },
    }
}
```

## Built-in Methods[​](#built-in-methods "Direct link to Built-in Methods")

### status[​](#status "Direct link to status")

The `status` method will return the status of the rpc server. This method response includes a high-level status, like `ready`, `compiling`, or `error`, as well as the set of logs accumulated during the initial compilation of the project. When the rpc server is in the `compiling` or `error` state, only built-in methods of the RPC server will be accepted.

**Example request**

```json
{
    "jsonrpc": "2.0",
    "method": "status",
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d"
}
```

**Example response**

```json
{
    "result": {
        "status": "ready",
        "error": null,
        "logs": [..],
        "timestamp": "2019-10-07T16:30:09.875534Z",
        "pid": 76715
    },
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "jsonrpc": "2.0"
}
```

### poll[​](#poll "Direct link to poll")

The `poll` endpoint will return the status, logs, and results (if available) for a running or completed task. The `poll` method requires a `request_token` parameter which indicates the task to poll a response for. The `request_token` is returned in the response of dbt tasks like `compile`, `run` and `test`.

**Parameters**:

* `request_token`: The token to poll responses for
* `logs`: A boolean flag indicating if logs should be returned in the response (default=false)
* `logs_start`: The zero-indexed log line to fetch logs from (default=0)

**Example request**

```json
{
    "jsonrpc": "2.0",
    "method": "poll",
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "params": {
        "request_token": "f86926fa-6535-4891-8d24-2cfc65d2a347",
        "logs": true,
        "logs_start": 0
    }
}
```

**Example Response**

```json
{
    "result": {
        "results": [],
        "generated_at": "2019-10-11T18:25:22.477203Z",
        "elapsed_time": 0.8381369113922119,
        "logs": [],
        "tags": {
            "command": "run --select my_model",
            "branch": "abc123"
        },
        "status": "success"
    },
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "jsonrpc": "2.0"
}
```

### ps[​](#ps "Direct link to ps")

The `ps` methods lists running and completed processes executed by the RPC server.

**Parameters**

* `completed`: If true, also return completed tasks (default=false)

**Example request:**

```json
{
    "jsonrpc": "2.0",
    "method": "ps",
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "params": {
        "completed": true
    }
}
```

**Example response:**

```json
{
    "result": {
        "rows": [
            {
                "task_id": "561d4a02-18a9-40d1-9f01-cd875c3ec56d",
                "request_id": "3db9a2fe-9a39-41ef-828c-25e04dd6b07d",
                "request_source": "127.0.0.1",
                "method": "run",
                "state": "success",
                "start": "2019-10-07T17:09:49.865976Z",
                "end": null,
                "elapsed": 1.107261,
                "timeout": null,
                "tags": {
                    "command": "run --select my_model",
                    "branch": "feature/add-models"
                }
            }
        ]
    },
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "jsonrpc": "2.0"
}
```

### kill[​](#kill "Direct link to kill")

The `kill` method will terminate a running task. You can find a `task_id` for a running task either in the original response which invoked that task, or in the results of the `ps` method.

**Example request**

```json
{
    "jsonrpc": "2.0",
    "method": "kill",
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "params": {
        "task_id": "{ the task id to terminate }"
    }
}
```

## Running dbt projects[​](#running-dbt-projects "Direct link to Running dbt projects")

The following methods make it possible to run dbt projects via the RPC server.

### Common parameters[​](#common-parameters "Direct link to Common parameters")

All RPC requests accept the following parameters in addition to the parameters listed:

* `timeout`: The max amount of time to wait before cancelling the request.
* `task_tags`: Arbitrary key/value pairs to attach to this task. These tags will be returned in the output of the `poll` and `ps` methods (optional).

### Running a task with CLI syntax[​](#running-a-task-with-cli-syntax "Direct link to Running a task with CLI syntax")

**Parameters:**

* `cli`: A dbt command (eg. `run --select abc+ --exclude +def`) to run (required)

```json
{
    "jsonrpc": "2.0",
    "method": "cli_args",
    "id": "<request id>",
    "params": {
        "cli": "run --select abc+ --exclude +def",
        "task_tags": {
            "branch": "feature/my-branch",
            "commit": "c0ff33b01"
        }
    }
}
```

Several of the following request types accept these additional parameters:

* `threads`: The number of [threads](https://docs.getdbt.com/docs/core/connect-data-platform/connection-profiles.md#understanding-threads) to use when compiling (optional)
* `select`: The space-delimited set of resources to execute (optional). (`models` is also supported on some request types for backwards compatibility.)
* `selector`: The name of a predefined [YAML selector](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md) that defines the set of resources to execute (optional)
* `exclude`: The space-delimited set of resources to exclude from compiling, running, testing, seeding, or snapshotting (optional)
* `state`: The filepath of artifacts to use when establishing [state](https://docs.getdbt.com/reference/node-selection/syntax.md#about-node-selection) (optional)

### Compile a project ([docs](https://docs.getdbt.com/reference/commands/compile.md))[​](#compile-a-project-docs "Direct link to compile-a-project-docs")

```json
{
	"jsonrpc": "2.0",
	"method": "compile",
	"id": "<request id>",
	"params": {
            "threads": "<int> (optional)",
            "select": "<str> (optional)",
            "exclude": "<str> (optional)",
            "selector": "<str> (optional)",
            "state": "<str> (optional)"
        }
}
```

### Run models ([docs](https://docs.getdbt.com/reference/commands/run.md))[​](#run-models-docs "Direct link to run-models-docs")

**Additional parameters:**

* `defer`: Whether to defer references to upstream, unselected resources (optional, requires `state`)

```json
{
	"jsonrpc": "2.0",
	"method": "run",
	"id": "<request id>",
	"params": {
            "threads": "<int> (optional)",
            "select": "<str> (optional)",
            "exclude": "<str> (optional)",
            "selector": "<str> (optional)",
            "state": "<str> (optional)",
            "defer": "<bool> (optional)"
        }
}
```

### Run tests ([docs](https://docs.getdbt.com/reference/commands/test.md))[​](#run-tests-docs "Direct link to run-tests-docs")

**Additional parameters:**

* `data`: If True, run data tests (optional, default=true)
* `schema`: If True, run schema tests (optional, default=true)

```json
{
	"jsonrpc": "2.0",
	"method": "test",
	"id": "<request id>",
	"params": {
            "threads": "<int> (optional)",
            "select": "<str> (optional)",
            "exclude": "<str> (optional)",
            "selector": "<str> (optional)",
            "state": "<str> (optional)",
            "data": "<bool> (optional)",
            "schema": "<bool> (optional)"
        }
}
```

### Run seeds ([docs](https://docs.getdbt.com/reference/commands/seed.md))[​](#run-seeds-docs "Direct link to run-seeds-docs")

**Parameters:**

* `show`: If True, show a sample of the seeded data in the response (optional, default=false)

```json
{
	"jsonrpc": "2.0",
	"method": "seed",
	"id": "<request id>",
	"params": {
            "threads": "<int> (optional)",
            "select": "<str> (optional)",
            "exclude": "<str> (optional)",
            "selector": "<str> (optional)",
            "show": "<bool> (optional)",
            "state": "<str> (optional)"
        }
}
```

### Run snapshots ([docs](https://docs.getdbt.com/docs/build/snapshots.md))[​](#run-snapshots-docs "Direct link to run-snapshots-docs")

```json
{
	"jsonrpc": "2.0",
	"method": "snapshot",
	"id": "<request id>",
	"params": {
            "threads": "<int> (optional)",
            "select": "<str> (optional)",
            "exclude": "<str> (optional)",
            "selector": "<str> (optional)",
            "state": "<str> (optional)"
        }
}
```

### Build ([docs](https://docs.getdbt.com/reference/commands/build.md))[​](#build-docs "Direct link to build-docs")

```json
{
	"jsonrpc": "2.0",
	"method": "build",
	"id": "<request id>",
	"params": {
            "threads": "<int> (optional)",
            "select": "<str> (optional)",
            "exclude": "<str> (optional)",
            "selector": "<str> (optional)",
            "state": "<str> (optional)",
            "defer": "<str> (optional)"
        }
}
```

### List project resources ([docs](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate))[​](#list-project-resources-docs "Direct link to list-project-resources-docs")

**Additional parameters:**

* `resource_types`: Filter selected resources by type
* `output_keys`: Specify which node properties to include in output

```json
{
	"jsonrpc": "2.0",
	"method": "ls",
	"id": "<request id>",
	"params": {
        "select": "<str> (optional)",
        "exclude": "<str> (optional)",
        "selector": "<str> (optional)",
        "resource_types": ["<list> (optional)"],
        "output_keys": ["<list> (optional)"],
    }
}
```

### Generate docs ([docs](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate))[​](#generate-docs-docs "Direct link to generate-docs-docs")

**Additional parameters:**

* `compile`: If True, compile the project before generating a catalog (optional, default=false)

```json
{
	"jsonrpc": "2.0",
	"method": "docs.generate",
	"id": "<request id>",
	"params": {
            "compile": "<bool> (optional)",
            "state": "<str> (optional)"
        }
}
```

## Compiling and running SQL statements[​](#compiling-and-running-sql-statements "Direct link to Compiling and running SQL statements")

### Compiling a query[​](#compiling-a-query "Direct link to Compiling a query")

This query compiles the SQL `select {{ 1 + 1 }} as id` (base64-encoded) against the rpc server:

rpc-spec.json

```json
{
    "jsonrpc": "2.0",
    "method": "compile_sql",
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "params": {
        "timeout": 60,
        "sql": "c2VsZWN0IHt7IDEgKyAxIH19IGFzIGlk",
        "name": "my_first_query"
    }
}
```

The resulting response will include a key called `compiled_sql` with a value of `'select 2'`.

### Executing a query[​](#executing-a-query "Direct link to Executing a query")

This query executes the SQL `select {{ 1 + 1 }} as id` (bas64-encoded) against the rpc server:

rpc-run.json

```json
{
    "jsonrpc": "2.0",
    "method": "run_sql",
    "id": "2db9a2fe-9a39-41ef-828c-25e04dd6b07d",
    "params": {
        "timeout": 60,
        "sql": "c2VsZWN0IHt7IDEgKyAxIH19IGFzIGlk",
        "name": "my_first_query"
    }
}
```

The resulting response will include a key called `table` with a value of `{'column_names': ['?column?'], 'rows': [[2.0]]}`

## Reloading the RPC Server[​](#reloading-the-rpc-server "Direct link to Reloading the RPC Server")

When the dbt RPC Server starts, it will load the dbt project into memory using the files present on disk at startup. If the files in the dbt project should change (either during development or in a deployment), the dbt RPC Server can be updated live without cycling the server process. To reload the files present on disk, send a "hangup" signal to the running server process using the Process ID (pid) of the running process.

### Finding the server PID[​](#finding-the-server-pid "Direct link to Finding the server PID")

To find the server PID, either fetch the `.result.pid` value from the `status` method response on the server, or use `ps`:

```text
# Find the server PID using `ps`:
ps aux | grep 'dbt-rpc serve' | grep -v grep
```

After finding the PID for the process (eg. 12345), send a signal to the running server using the `kill` command:

```text
kill -HUP 12345
```

When the server receives the HUP (hangup) signal, it will re-parse the files on disk and use the updated project code when handling subsequent requests.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
