# About dbt system command

`dbt system` provides commands for managing your dbt installation. v2 is a single compiled binary with no dependencies on other libraries. Once installed, `dbt system` lets you update, uninstall, and manage drivers directly without pip, brew, or a Python environment.

`dbt system update` and `dbt system uninstall` are only supported for [CDN installations](../../docs/local/install-dbt.md) of dbt v2. If you installed dbt v2 via Pip, Homebrew, or Winget, you'll need to update or uninstall dbt from those tools.

dbt v1 has no equivalent.

## Usage

```shell
dbt system <subcommand> [flags]
```

## Subcommands

| Subcommand                   | Description                                       |
| ---------------------------- | ------------------------------------------------- |
| `dbt system update`          | Update dbt to the latest (or a specified) version |
| `dbt system uninstall`       | Remove dbt v2 from your system                    |
| `dbt system install-drivers` | Pre-install all supported ADBC adapter drivers    |

## dbt system update

`dbt system update` updates the CLI binary in place. By default, it updates to the latest stable release. You can also target a named channel or a specific version number.

```shell
dbt system update [flags]
```

### Options

| Flag                | Description                                                                                                                                |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `--version VERSION` | Update to a specific version (for example, `2.0.1`) or a named channel (`stable`, `preview`, `nightly`). Defaults to the `stable` release. |
| `--package PACKAGE` | Which package to update. Accepts `dbt` or `all`. Defaults to `dbt`.                                                                        |

### Version channels

| Channel   | Description                                                                         | Stability                                                                                                                                                         |
| --------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `stable`  | The latest generally available (GA) release                                         | ✅ Recommended for most users.                                                                                                                                    |
| `preview` | The latest prerelease (alpha, beta, or release candidate) of the next minor version | ⚠️ Passed release testing, but behavior can still change before GA. Refer to [Prerelease stages](../../docs/dbt-versions.md#prerelease-stages). |
| `nightly` | The latest nightly build from the main branch                                       | ❌ For testing upcoming changes only. Not covered by prerelease or regression testing.                                                                            |

### Examples

Update to the latest stable release:

```shell
dbt system update
```

Update to a specific version:

```shell
dbt system update --version 2.0.1
```

Update to the preview channel:

```shell
dbt system update --version preview
```

## dbt system uninstall

`dbt system uninstall` removes the CLI binary from your system. On macOS and Linux, it downloads and runs the official `uninstall.sh` script. On Windows, a PowerShell script handles removal after the current process exits (to release the file lock on the running executable).

```shell
dbt system uninstall
```

This command takes no additional flags.

## dbt system install-drivers

`dbt system install-drivers` downloads and caches all supported ADBC adapter driver libraries (`.dylib`, `.so`, `.dll`) for every supported data platform at once. Normally, dbt downloads drivers on first use when you run a command against a new data platform. Use `dbt system install-drivers` to pre-populate the cache (for example, before going offline or switching between projects that target different warehouses).

```shell
dbt system install-drivers
```

This command takes no additional flags.

Networking note

`dbt system install-drivers` downloads files from `https://public.cdn.getdbt.com`. If your environment restricts outbound network access, make sure this endpoint is allowlisted. See [dbt v2 networking requirements](../../docs/local/dbt-networking-requirements.md) for the full list of endpoints.

## Related commands

* [`dbt debug`](./debug.md) — Test your dbt project and connection configuration.
* [dbt v2 networking requirements](../../docs/local/dbt-networking-requirements.md) — Review outbound endpoints required by dbt v2.
