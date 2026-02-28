# Caveats to state comparison

The [`state:` selection method](https://docs.getdbt.com/reference/node-selection/methods.md#state) is a powerful feature, with a lot of underlying complexity. Below are a handful of considerations when setting up automated jobs that leverage state comparison.

### Seeds[​](#seeds "Direct link to Seeds")

dbt stores a file hash of seed files that are <1 MiB in size. If the contents of these seeds is modified, the seed will be included in `state:modified`.

If a seed file is >1 MiB in size, dbt cannot compare its contents and will raise a warning as such. Instead, dbt will use only the seed's file path to detect changes. If the file path has changed, the seed will be included in `state:modified`; if it hasn't, it won't.

### Macros[​](#macros "Direct link to Macros")

dbt will mark modified any resource that depends on a changed macro, or on a macro that depends on a changed macro.

### Vars[​](#vars "Direct link to Vars")

If a model uses a `var` or `env_var` in its definition, dbt is unable to identify that lineage in such a way that it can include the model in `state:modified` because the `var` or `env_var` value has changed. It's likely that the model will be marked modified if the change in variable results in a different configuration.

### Tests[​](#tests "Direct link to Tests")

The command `dbt test -s state:modified` will include both:

* tests that select from a new/modified resource
* tests that are themselves new or modified

As long as you're adding or changing tests at the same time that you're adding or changing the resources (models, seeds, snapshots) they select from, all should work the way you expect with "simple" state selection:

```shell
dbt run -s "state:modified"
dbt test -s "state:modified"
```

This can get complicated, however. If you add a new test without modifying its underlying model, or add a test that selects from a new model and an old unmodified one, you may need to test a model without having first run it.

You can defer upstream references when testing. For example, if a test selects from a model that doesn't exist as a database object in your current environment, dbt will look to the other environment instead—the one defined in your state manifest. This enables you to use "simple" state selection without risk of query failure, but it may have some surprising consequences for tests with multiple parents. For instance, if you have a `relationships` test that depends on one modified model and one unmodified model, the test query will select from data "across" two different environments. If you limit or sample your data in development and CI, it may not make much sense to test for referential integrity, knowing there's a good chance of mismatch.

If you're a frequent user of `relationships` tests or data tests, or frequently find yourself adding tests without modifying their underlying models, consider tweaking the selection criteria of your CI job. For instance:

```shell
dbt run -s "state:modified"
dbt test -s "state:modified" --exclude "test_name:relationships"
```

### Overwrites the `manifest.json`[​](#overwrites-the-manifestjson "Direct link to overwrites-the-manifestjson")

<!-- -->

dbt overwrites the `manifest.json` file during parsing, which means when you reference `--state` from the `target/ directory`, you may encounter a warning indicating that the saved manifest wasn't found.

[![Saved manifest not found error](/img/docs/reference/saved-manifest-not-found.png?v=2 "Saved manifest not found error")](#)Saved manifest not found error

During the next job run, dbt follows a sequence of steps that lead to the issue. First, it overwrites `target/manifest.json` before it can be used for change detection. Then, when dbt tries to read `target/manifest.json` again to detect changes, it finds none because the previous state has already been overwritten/erased.

Avoid setting `--state` and `--target-path` to the same path with state-dependent features like `--defer` and `state:modified` as it can lead to non-idempotent behavior and won't work as expected.

#### Recommendation[​](#recommendation "Direct link to Recommendation")

<!-- -->

To prevent the `manifest.json` from being overwritten before dbt reads it for change detection, update your workflow using one of these methods:

* Move the `manifest.json` to a dedicated folder (for example `state/`) after dbt generates it in the `target/ folder`. This makes sure dbt references the correct saved state instead of comparing the current state with the just-overwritten version. It also avoids issues caused by setting `--state` and `--target-path` to the same location, which can lead to non-idempotent behavior.

* Write the manifest to a different `--target-path` in the build stage (where dbt would generate the `target/manifest.json`) or before it gets overwritten during job execution to avoid issues with change detection. This allows dbt to detect changes instead of comparing the current state with the just-overwritten version.

* Pass the `--no-write-json` flag: `dbt ls --no-write-json --select state:modified --state target`: during the reproduction stage.

### False positives[​](#false-positives "Direct link to False positives")

<!-- -->

### Final note[​](#final-note "Direct link to Final note")

State comparison is complex. We hope to reach eventual consistency between all configuration options, as well as providing users with the control they need to reliably return all modified resources, and only the ones they expect. If you're interested in learning more, read [open issues tagged "state"](https://github.com/dbt-labs/dbt-core/issues?q=is%3Aopen+is%3Aissue+label%3Astate) in the dbt repository.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About state in dbt](https://docs.getdbt.com/reference/node-selection/state-selection.md)
* [Configure state selection](https://docs.getdbt.com/reference/node-selection/configure-state.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
