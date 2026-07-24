# dbt licensing

v2 has the following distributions today, all free to install and run.

| Distribution | Package    | Use it when                                                                          |
| ------------ | ---------- | ------------------------------------------------------------------------------------ |
| Fusion       | `dbt`      | You want the recommended v2 experience, with Fusion installed by default.            |
| dbt Core 2.0 | `dbt-core` | Your organization has a strict requirement to use the Apache 2.0 open-source runtime |

If you have a older project that isn’t ready to move to v2, continue using `dbt-core` v1.x for compatibility. For new or upgraded projects, we recommend [upgrading to v2](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md?version=2.0#dbt-fusion-engine%20in%20the%20dbt%20platform).

## Which one should I use?[​](#which-one-should-i-use "Direct link to Which one should I use?")

For most people: Fusion. It has more [capabilities](https://docs.getdbt.com/docs/fusion/fusion-availability.md?version=2.0#what-you-get-with-fusion) out of the box than dbt Core 2.0 — including a built-in high-performance SQL linter — even if you never create a dbt account.

We recommend everyone to just [install dbt](https://docs.getdbt.com/docs/local/install-dbt.md) and get Fusion by default.

Typically you'd choose dbt Core 2.0 directly only if you're in one of two specific situations: your organization's license policy requires a strict open-source distribution, or you're building something custom on top of the OSS code itself.

Already running dbt Core v1.x? You don't have to move to v2 — it's still fully supported. Over time, new capabilities will land in v2 only, so most people will eventually want to [upgrade ](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md?version=2.0#dbt-fusion-engine%20in%20the%20dbt%20platform).

To check which distribution you're using, run `dbt --version` in the command line.

## What changed, and what didn't[​](#what-changed-and-what-didnt "Direct link to What changed, and what didn't")

**Changed:**

* v2 is available through two distributions: Fusion and dbt Core 2.0.
* dbt Core 2.0 is the new Apache 2.0 open-source distribution for v2, powered by the shared Rust engine code now available in `dbt-core`.
* Fusion builds on dbt Core 2.0 and extends it with additional proprietary capabilities under the dbt Product Licensing Agreement.

**Unchanged:**

* dbt Core v1.x is still fully available and still Apache 2.0.
* Fusion is still completely free to use, with some features unlocked by a free login or a paid dbt platform account — not required for any distribution.
* Contributing to dbt is still open to everyone.

<!-- -->

## Licensing details[​](#licensing-details "Direct link to Licensing details")

[dbt Core](https://github.com/dbt-labs/dbt-core) is released under the [Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0). Fusion is proprietary to dbt Labs, made available under the [dbt Product Licensing Agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement).

For the full breakdown of what's permitted under each license — source visibility, contributions, modifications, self-hosting, and redistribution — see the [dbt Licensing FAQ](https://www.getdbt.com/licenses-faq).

dbt platform is a separate hosted product governed by its own [terms of service](https://www.getdbt.com/terms-of-use). Also not to be confused with dbt platform [licenses](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md).
