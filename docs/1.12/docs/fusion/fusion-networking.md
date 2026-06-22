# Networking requirements [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Fusion requires outbound HTTPS access to several endpoints depending on your usage. This page describes each requirement and provides guidance for enterprise environments that restrict outbound traffic.

The following table summarizes all endpoints. See each section below for details.

| Resource                                  | URL                                            | Required for                |
| ----------------------------------------- | ---------------------------------------------- | --------------------------- |
| [Adapter drivers](#adapter-drivers)       | `https://public.cdn.getdbt.com`                | All users                   |
| [Telemetry](#telemetry)                   | `https://p.vx.dbt.com`                         | All users (can be disabled) |
| [Manifest downloads](#manifest-downloads) | Cloud provider storage URLs (varies by region) | dbt platform users only     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Adapter drivers[​](#adapter-drivers "Direct link to Adapter drivers")

The Fusion binary does *not* bundle database drivers. Instead, Fusion automatically downloads the correct [ADBC](https://arrow.apache.org/adbc/) driver for your data platform the first time you run a dbt command (such as `dbt run`, `dbt debug`, or `dbt compile`). Fusion detects which driver you need based on your `profiles.yml` configuration and downloads it from the dbt Labs CDN. Fusion distributes all checksums with the binary itself to guarantee authenticity of the downloaded drivers.

Adapter driver downloads require outbound HTTPS access to the dbt CDN:

| Resource            | URL                             | Purpose                                                                                                                     |
| ------------------- | ------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Adapter drivers** | `https://public.cdn.getdbt.com` | Downloads ADBC adapter driver libraries (`.dylib`, `.so`, `.dll`) on first use or when running `dbt system install-drivers` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

info

Fusion handles driver download automatically on first use. The `dbt system install-drivers` command downloads **all** supported drivers (Snowflake, BigQuery, Postgres, Databricks, Redshift, DuckDB, and Salesforce) at once. This is useful if you work across multiple data platforms and want to pre-cache every driver before going offline or switching projects.

### Enterprise proxy considerations[​](#enterprise-proxy-considerations "Direct link to Enterprise proxy considerations")

Adapter drivers are native shared libraries (`.dylib` on macOS, `.so` on Linux, `.dll` on Windows). Some enterprise proxy filters and security tools classify these file types as executables and may block the download — even if you allowlist `public.cdn.getdbt.com` at the domain level.

If your organization's proxy blocks adapter driver downloads, work with your IT team to ensure both:

1. You allowlist the domain `public.cdn.getdbt.com`.
2. Content inspection rules permit downloading native library file types (`.dylib`, `.so`, `.dll`) from that domain.

If you cannot change your proxy configuration, see [Restricted network installation](#restricted-network-installation).

### Restricted network installation[​](#restricted-network-installation "Direct link to Restricted network installation")

If your environment cannot access `public.cdn.getdbt.com` for adapter driver downloads, you can pre-build a bundle of the Fusion binary and the adapter drivers into a single `.tar.gz` or Docker image and host it on an internally approved fileshare.

For supported adapters, refer to [Fusion requirements](https://docs.getdbt.com/docs/fusion/supported-features.md#requirements).

## Telemetry[​](#telemetry "Direct link to Telemetry")

Fusion sends anonymous usage [telemetry](https://docs.getdbt.com/docs/fusion/telemetry.md) to help improve the product. If the telemetry endpoint is unreachable (for example, blocked by a firewall or proxy), Fusion logs errors on each invocation.

| Resource      | URL                    | Purpose                          |
| ------------- | ---------------------- | -------------------------------- |
| **Telemetry** | `https://p.vx.dbt.com` | Sends anonymous usage statistics |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To suppress these errors without allowlisting the URL, disable anonymous telemetry by setting the environment variable:

```shell
export DBT_SEND_ANONYMOUS_USAGE_STATS=false
```

You can also add this to your `.env` file in your project root:

```env
DBT_SEND_ANONYMOUS_USAGE_STATS=false
```

For more details on `.env` file usage, refer to [Environment variables](https://docs.getdbt.com/docs/local/configure-environment-variables.md).

## Manifest downloads (dbt platform only) enterprise[​](#manifest-downloads "Direct link to manifest-downloads")

For [dbt platform](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md) customers using Fusion locally, Fusion downloads production manifests from dbt platform to enable features like [deferral](https://docs.getdbt.com/reference/node-selection/defer.md) and [cross-project references](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md). The [cloud storage provider](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) hosting your dbt platform cell serves these manifests via **pre-signed URLs**.

The specific hostnames depend on your dbt platform deployment region and the underlying cloud provider. To ensure Fusion can download manifests, allowlist the appropriate storage domain for your region:

| Cloud provider           | URL pattern                               | Example                                         |
| ------------------------ | ----------------------------------------- | ----------------------------------------------- |
| **AWS (S3)**             | `https://s3.<region>.amazonaws.com`       | `https://s3.ap-northeast-1.amazonaws.com` (JP1) |
| **Azure (Blob Storage)** | `https://<account>.blob.core.windows.net` | `https://prodeu2.blob.core.windows.net` (EU2)   |
| **GCP (Cloud Storage)**  | `https://storage.googleapis.com`          | `storage.googleapis.com`                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Because pre-signed URLs contain region and account-specific hostnames that may change over time, we recommend allowlisting the **base storage domain** for your cloud provider rather than individual URLs:

* **AWS** — `s3.*.amazonaws.com`
* **Azure** — `*.blob.core.windows.net`
* **GCP** — `storage.googleapis.com`

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
