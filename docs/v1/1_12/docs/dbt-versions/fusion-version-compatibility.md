# Version compatibility [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

The dbt Fusion engine, its language server (LSP), and the dbt VS Code extension work together to power local development. This page explains how their versions relate, which combinations are compatible, and how to verify you have matching binaries.

## How versioning works[​](#how-versioning-works "Direct link to How versioning works")

Fusion ships as a single binary that provides both the command-line interface (CLI) and the language server (LSP). Because the CLI and LSP come from the *same* binary, they always share the same version and can't be mismatched.

The dbt VS Code extension keeps its own release version because it follows the VS Code Marketplace release lifecycle. To make compatibility explicit, each extension release declares the range of dbt Fusion engine versions it supports. When the extension starts the LSP, it checks the installed Fusion version against that range and surfaces an [actionable error](#troubleshooting-version-issues) if they don't match.

| Component                            | Example versioning                                                                  |
| ------------------------------------ | ----------------------------------------------------------------------------------- |
| dbt Fusion engine binary (CLI + LSP) | `2.0.0-preview.N`                                                                   |
| dbt VS Code extension                | The extension version (for example, `0.36.0`), with compatible Fusion version range |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Compatibility matrix[​](#compatibility-matrix "Direct link to Compatibility matrix")

The following table maps each dbt VS Code extension version to the dbt Fusion engine versions it supports. If you let the extension download and manage Fusion for you, it always installs a compatible version automatically — you only need this table if you install or pin binaries manually.

| dbt VS Code extension | Min Fusion version | Max Fusion version | Checksums            | Notes   |
| --------------------- | ------------------ | ------------------ | -------------------- | ------- |
| 0.36.x                | 2.0.0-preview\.90  | —                  | `checksums-0.36.txt` | Current |
| 0.35.x                | 2.0.0-preview\.80  | 2.0.0-preview\.89  | `checksums-0.35.txt` | -       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

A dash (-) in the **Max** column means there is no upper bound yet. Any Fusion release at or above the minimum is supported.

## Check your installed versions[​](#check-your-installed-versions "Direct link to Check your installed versions")

To confirm your setup is within the supported range, check the version of the dbt Fusion engine you have installed:

```shell
dbt --version
```

For a machine-readable summary you can paste into a bug report or parse in tooling, add the `--format json` flag:

```shell
dbt --version --format json
```

For details on the full output, refer to [About dbt --version](../../reference/commands/version.md).

## Verify binaries for manual and air-gapped installs[​](#verify-binaries-for-manual-and-air-gapped-installs "Direct link to Verify binaries for manual and air-gapped installs")

If you install Fusion manually in an air-gapped or firewall-restricted environment where the extension can't download binaries for you, use the [compatibility matrix](#compatibility-matrix) to pick a version that matches your dbt VS Code extension, then verify the download before you distribute it.

1. From the compatibility matrix, note the Fusion version range for your extension version.

2. Download the matching Fusion binary and its checksum file.

3. Verify the checksum before installing or distributing the binary:

   * Mac / Linux
   * Windows (PowerShell)

   ```shell
   shasum -a 256 -c checksums-0.36.txt
   ```

   ```powershell
   Get-FileHash .\dbt.exe -Algorithm SHA256
   ```

   Compare the printed hash against the value in the checksum file.

4. Point the dbt VS Code extension at the verified binary using the `dbt.fusionPath` setting. Refer to [dbt extension settings](../configure-dbt-extension.md#dbt-extension-settings).

For network and proxy requirements, and how to pre-build an offline bundle, refer to [Networking requirements](../local/fusion-networking-requirements.md#restricted-network-installation).

## Known-bad releases[​](#known-bad-releases "Direct link to Known-bad releases")

If a shipped Fusion release is later found to contain a regression, dbt Labs flags it as a known-bad release. When you have a flagged version installed, the dbt VS Code extension shows a warning notification telling you which version to update to, even if that version is otherwise within the supported range.

Air-gapped users who don't have outbound network access can distribute the known-bad releases manifest alongside their binary bundle and point the extension at the local copy with the `dbt.badReleasesManifestPath` setting. Refer to [dbt extension settings](../configure-dbt-extension.md#dbt-extension-settings).

## Troubleshooting version issues[​](#troubleshooting-version-issues "Direct link to Troubleshooting version issues")

If the extension can't find, start, or verify a compatible Fusion binary, it surfaces an actionable error with a link or button to resolve it. Refer to the [dbt VS Code extension troubleshooting](../sign-in-dbt-extension.md#troubleshooting) section for the full list of messages and fixes.
