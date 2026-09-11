# dbt v2 networking requirements [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt v2 requires outbound HTTPS access to several endpoints depending on your usage. This page describes each requirement and provides guidance for enterprise environments that restrict outbound traffic.

The following table summarizes all endpoints. See each section below for details.

| Resource                                  | URL                                            | Required for                |
| ----------------------------------------- | ---------------------------------------------- | --------------------------- |
| [Adapter drivers](#adapter-drivers)       | `https://public.cdn.getdbt.com`                | All users                   |
| [Telemetry](#telemetry)                   | `https://p.vx.dbt.com`                         | All users (can be disabled) |
| [Manifest downloads](#manifest-downloads) | Cloud provider storage URLs (varies by region) | dbt platform users only     |

## Adapter drivers

The dbt v2 binary does *not* bundle database drivers. Instead, dbt v2 automatically downloads the correct [ADBC](https://arrow.apache.org/adbc/) driver for your data platform the first time you run a dbt command (such as `dbt run`, `dbt debug`, or `dbt compile`). dbt v2 detects which driver you need based on your `profiles.yml` configuration and downloads it from the dbt Labs CDN. dbt v2 distributes all checksums with the binary itself to guarantee authenticity of the downloaded drivers.

Adapter driver downloads require outbound HTTPS access to the dbt CDN:

| Resource            | URL                             | Purpose                                                                                                                     |
| ------------------- | ------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Adapter drivers** | `https://public.cdn.getdbt.com` | Downloads ADBC adapter driver libraries (`.dylib`, `.so`, `.dll`) on first use or when running `dbt system install-drivers` |

info

dbt v2 handles driver download automatically on first use. The `dbt system install-drivers` command downloads **all** supported drivers (Snowflake, BigQuery, Postgres, Databricks, Redshift, DuckDB, and Salesforce) at once. This is useful if you work across multiple data platforms and want to pre-cache every driver before going offline or switching projects.

### Enterprise proxy considerations

Adapter drivers are native shared libraries (`.dylib` on macOS, `.so` on Linux, `.dll` on Windows). Some enterprise proxy filters and security tools classify these file types as executables and may block the download — even if you allowlist `public.cdn.getdbt.com` at the domain level.

If your organization's proxy blocks adapter driver downloads, work with your IT team to ensure both:

1. You allowlist the domain `public.cdn.getdbt.com`.
2. Content inspection rules permit downloading native library file types (`.dylib`, `.so`, `.dll`) from that domain.

If you cannot change your proxy configuration, see [Restricted network installation](#restricted-network-installation).

### Restricted network installation

If your environment cannot access `public.cdn.getdbt.com` for adapter driver downloads, you can pre-build a bundle of the dbt v2 binary and the adapter drivers into a single `.tar.gz` or Docker image and host it on an internally approved fileshare.

When you assemble a bundle for air-gapped or firewall-restricted machines:

1. Use the [version compatibility matrix](../dbt-versions/dbt-version-compatibility.md#compatibility-matrix) to choose a dbt v2 version that matches the dbt VS Code extension your users run.
2. Verify the binary against its published SHA-256 checksum before distributing it. Refer to [Verify binaries for manual and air-gapped installs](../dbt-versions/dbt-version-compatibility.md#verify-binaries-for-manual-and-air-gapped-installs).
3. Point the dbt VS Code extension at the bundled binary with the `dbt.fusionPath` setting. Refer to [dbt extension settings](../configure-dbt-extension.md#dbt-extension-settings).

For supported adapters, refer to [v2 requirements](../dbt/supported-features.md#requirements).

## Telemetry

dbt v2 sends anonymous usage statistics to help improve the product. If the telemetry endpoint is unreachable (for example, blocked by a firewall or proxy), dbt v2 logs errors on each invocation.

| Resource      | URL                    | Purpose                          |
| ------------- | ---------------------- | -------------------------------- |
| **Telemetry** | `https://p.vx.dbt.com` | Sends anonymous usage statistics |

To suppress these errors without allowlisting the URL, disable telemetry using the options described in [Anonymous usage stats](../../reference/global-configs/usage-stats.md).

## Manifest downloads (dbt platform only) [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

For [dbt platform](../platform/about-platform/dbt-platform-features.md) customers using dbt v2 locally, dbt v2 downloads production manifests from dbt platform to enable features like [deferral](../../reference/node-selection/defer.md) and [cross-project references](../mesh/govern/project-dependencies.md). The [cloud storage provider](../platform/about-platform/access-regions-ip-addresses.md) hosting your dbt platform cell serves these manifests via **pre-signed URLs**.

The specific hostnames depend on your dbt platform deployment region and the underlying cloud provider. To ensure dbt v2 can download manifests, allowlist the appropriate storage domain for your region:

| Cloud provider           | URL pattern                               | Example                                         |
| ------------------------ | ----------------------------------------- | ----------------------------------------------- |
| **AWS (S3)**             | `https://s3.<region>.amazonaws.com`       | `https://s3.ap-northeast-1.amazonaws.com` (JP1) |
| **Azure (Blob Storage)** | `https://<account>.blob.core.windows.net` | `https://prodeu2.blob.core.windows.net` (EU2)   |
| **GCP (Cloud Storage)**  | `https://storage.googleapis.com`          | `storage.googleapis.com`                        |

Because pre-signed URLs contain region and account-specific hostnames that may change over time, we recommend allowlisting the **base storage domain** for your cloud provider rather than individual URLs:

* **AWS** — `s3.*.amazonaws.com`
* **Azure** — `*.blob.core.windows.net`
* **GCP** — `storage.googleapis.com`
