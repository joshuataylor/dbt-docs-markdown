# Product lifecycles

dbt Labs manages the lifecycle of features across the dbt platform, dbt Core, and the dbt Fusion engine. Each feature is assigned a lifecycle status that describes its stability, support level, and availability. Use the tabs below to find the lifecycle stages for the product you're using.

Service level objective (SLO) support varies between products and lifecycles.

* dbt platform
* dbt Core v1 and v2
* dbt Fusion engine

dbt platform features adhere to the following lifecycle path:

**Beta**

In active development. May not be fully stable and breaking changes can occur. Documentation may be incomplete, technical support is limited, and SLOs may not apply. Download the [Beta Terms and Conditions](https://docs.getdbt.com/assets/files/beta-tc-740ff696113c89c38a96bb70b968775e.pdf) for details. If marked `Private`, access must be enabled by dbt Labs.

**Preview**

Stable and functionally ready for production. Planned additions or non-backward-compatible changes may still occur before GA. Includes documentation, technical support, and SLOs. Available at no extra cost, though may become paid at GA. If marked `Private`, access must be enabled by dbt Labs.

**Generally available (GA)**

Stable features available to all qualified dbt accounts. SLOs, documentation, and technical support apply. Feature availability may depend on your environment's dbt version. Use a supported [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) to receive the latest GA features.

**Deprecated**

No longer being actively developed or enhanced. Features continue to function as-is and documentation remains available until the removal date. Technical support no longer applies.

**Removed**

No longer available on the platform in any capacity.

dbt Core releases follow semantic versioning. Read more in [About dbt versions](https://docs.getdbt.com/docs/dbt-versions.md). dbt Core v1 and v2 releases both adhere to the following lifecycle path:

**Undocumented**

dbt Core is an open source product, and the codebase may have visibility into internal, non-contracted, or intentionally undocumented functionality. Not considered part of the release's product surface area.

**Unreleased**

Planned for the next minor version prerelease. No commitments on behavior or implementation. Maintainers reserve the right to change or remove it entirely.

**Alpha**

No commitments on behavior or implementation and not intended for any production work. Use at your own discretion. Maintainers reserve the right to change or remove it entirely.

**Beta**

First glimpse of net-new features in an upcoming release. Code should work without regressions, but new features may be incomplete or have known edge cases. Changes are not locked and maintainers may still alter or remove them.

**Release Candidate**

A 2-week window for production-level testing before final release. Features are expected to ship as-is, though maintainers may still address significant bugs before the final release.

**Generally Available**

Ready for use in production.

**Deprecated**

No longer actively developed or enhanced. Continues to function as-is until its removal date.

The dbt Fusion engine and [VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) are in various stages of development depending on deployment type. See the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates, and our [Path to GA](https://docs.getdbt.com/blog/dbt-fusion-engine-path-to-ga) blog post for what's required to reach GA.

**Beta**

Available to select customers only. Still in development, incomplete, and not fully stable. Breaking changes may occur, documentation may be limited, and SLOs may not apply. Download the [Beta Terms and Conditions](https://docs.getdbt.com/assets/files/beta-tc-740ff696113c89c38a96bb70b968775e.pdf) for details.

**Preview**

Stable and functionally ready for production deployments using supported features that don't depend on deprecated functionality.

**Generally available (GA)**

Learn what's required for the dbt Fusion engine to reach GA in our [Path to GA](https://docs.getdbt.com/blog/dbt-fusion-engine-path-to-ga) blog post.
