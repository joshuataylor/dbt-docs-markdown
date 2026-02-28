# Upgrading to v1.4

### Resources[​](#resources "Direct link to Resources")

* [Changelog](https://github.com/dbt-labs/dbt-core/blob/1.4.latest/CHANGELOG.md)
* [dbt Core CLI Installation guide](https://docs.getdbt.com/docs/core/installation-overview.md)
* [Cloud upgrade guide](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md)

**Final release:** January 25, 2023

dbt Core v1.4 is a "behind-the-scenes" release. We've been hard at work rebuilding `dbt-core` internals on top of more-solid foundations, to enable an exciting year of new feature development. Check out the [v1.5 milestone](https://github.com/dbt-labs/dbt-core/milestone/82) in GitHub for a preview of what's planned for April.

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

dbt Labs is committed to providing backward compatibility for all versions 1.x. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

### For consumers of dbt artifacts (metadata)[​](#for-consumers-of-dbt-artifacts-metadata "Direct link to For consumers of dbt artifacts (metadata)")

The manifest schema version has updated to `v8`. These changes are relevant for people who parse or analyze the contents of the `manifest.json` file, or who have custom code accessing the [`model`](https://docs.getdbt.com/reference/dbt-jinja-functions/model.md) or [`graph`](https://docs.getdbt.com/reference/dbt-jinja-functions/graph.md) variables, for example, `{{ model.root_path }}`.

Relevant changes:

* The `root_path` attribute has been removed for non-seed nodes to reduce duplicative information.
* Unused attributes have been removed from seed nodes (including `depends_on.nodes`), and from `macros` (including `tags`).
* The `unique_id` of docs blocks now start with `doc` for consistency with other resource types.

### For maintainers of adapter plugins[​](#for-maintainers-of-adapter-plugins "Direct link to For maintainers of adapter plugins")

> **TL;DR** Not much heavy lifting for this minor version. We anticipate more work for `1.5.0`. We plan to release betas early & often, and provide guidance on upgrading.

The high-level changes are:

* Add support for Python 3.11
* Rename/replace deprecated exception functions
* Add support for Incremental Predicates (if applicable)
* Make use of new adapter-zone tests

For more detailed information and to ask any questions, please visit [dbt Core/discussions/6624](https://github.com/dbt-labs/dbt-core/discussions/6624).

## New and changed documentation[​](#new-and-changed-documentation "Direct link to New and changed documentation")

* [**Events and structured logging**](https://docs.getdbt.com/reference/events-logging.md): dbt's event system got a makeover. Expect more consistency in the availability and structure of information, backed by type-safe event schemas.
* [**Python support**](https://docs.getdbt.com/faqs/Core/install-python-compatibility.md): Python 3.11 was released in October 2022. It is officially supported in dbt-core v1.4, although full support depends also on the adapter plugin for your data platform. According to the Python maintainers, "Python 3.11 is between 10-60% faster than Python 3.10." We encourage you to try [`dbt parse`](https://docs.getdbt.com/reference/commands/parse.md) with dbt Core v1.4 + Python 3.11, and compare the timing with dbt Core v1.3 + Python 3.10. Let us know what you find!
* [**Metrics**](https://docs.getdbt.com/docs/build/build-metrics-intro.md): `time_grain` is optional, to provide better ergonomics around metrics that aren't time-bound.
* **dbt-Jinja context:** The [local\_md5](https://docs.getdbt.com/reference/dbt-jinja-functions/local_md5.md) context method will calculate an [MD5 hash](https://en.wikipedia.org/wiki/MD5) for use *within* dbt. (Not to be confused with SQL md5!)
* [**Exposures**](https://docs.getdbt.com/docs/build/exposures.md) can now depend on `metrics`.
* [**"Tarball" packages**](https://docs.getdbt.com/docs/build/packages.md#internally-hosted-tarball-URL): Some organizations have security requirements to pull resources only from internal services. To address the need to install packages from hosted environments (such as Artifactory or cloud storage buckets), it's possible to specify any accessible URL where a compressed dbt package can be downloaded.
* [**Granular "warn error" configuration**](https://docs.getdbt.com/reference/global-configs/warnings.md): Thanks to a full cleanup and consolidation of warning and exception classes within `dbt-core`, it is now possible to define a more granular `--warn-error-options` configuration that specifies the exact warnings you do (or don't) want dbt to treat as errors.
* [**Deferral**](https://docs.getdbt.com/reference/node-selection/defer.md#favor-state) supports an optional configuration, `--favor-state`.

### Advanced configurations for incremental models[​](#advanced-configurations-for-incremental-models "Direct link to Advanced configurations for incremental models")

* [**`incremental_predicates`** config](https://docs.getdbt.com/docs/build/incremental-strategy.md#about-incremental_predicates) is now supported on the most popular adapters, enabling greater flexibility when tuning performance in `merge` and `delete` statements against large datasets.
* **BigQuery:** The `insert_overwrite` incremental strategy supports a new (old) mechanism, [`time_ingestion_partitioning`](https://docs.getdbt.com/reference/resource-configs/bigquery-configs.md#partitioning-by-an-ingestion-date-or-timestamp) + [`copy_partitions`](#copying-ingestion-time-partitions), that can yield significant savings in cost + time for large datasets.

### Updates to Python models[​](#updates-to-python-models "Direct link to Updates to Python models")

* Python models are [configured to materialize](https://docs.getdbt.com/docs/build/python-models.md) as `table` by default.
* Python models [running on Snowpark](https://docs.getdbt.com/docs/build/python-models.md) will use "anonymous" stored procedures by default, enabling a small speedup and a cleaner query history.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
