# About dbt snapshot command

The `dbt snapshot` command executes the [Snapshots](https://docs.getdbt.com/docs/build/snapshots.md) defined in your project.

dbt will look for Snapshots in the `snapshot-paths` paths defined in your `dbt_project.yml` file. By default, the `snapshot-paths` path is `snapshots/`.

**Usage:**

```text
$ dbt snapshot --help
usage: dbt snapshot [-h] [--profiles-dir PROFILES_DIR]
                                     [--profile PROFILE] [--target TARGET]
                                     [--vars VARS] [--bypass-cache]
                                     [--threads THREADS]
                                     [--select SELECTOR [SELECTOR ...]]
                                     [--exclude EXCLUDE [EXCLUDE ...]]

optional arguments:
  --select SELECTOR [SELECTOR ...]
                        Specify the snapshots to include in the run.
  --exclude EXCLUDE [EXCLUDE ...]
                        Specify the snapshots to exclude in the run.
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
