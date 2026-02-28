# How is state-aware orchestration different from using selectors in dbt Core?

In dbt Core, running with the selectors `state:modified+` and `source_status:fresher+` builds models that either:

* Have changed since the prior run (`state:modified+`)
* Have upstream sources that are fresher than in the prior run (`source_status:fresher+`)

Instead of relying only on these selectors and prior-run artifacts, state-aware orchestration decides whether to rebuild a model based on:

* Compiled SQL diffs that ignore non-meaningful changes like whitespace and comments
* Upstream data changes at runtime and model-level freshness settings
* Shared state across jobs

While dbt Core uses selectors like `state:modified+` and `source_status:fresher+` to decide what to build *only for a single run in a single job*, state-aware orchestration with Fusion maintains a *shared, real-time model state across every job in the environment* and uses that state to determine whether a model’s code or upstream data have actually changed before rebuilding. This ensures dbt only rebuilds models when something has changed, no matter which job runs them.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
