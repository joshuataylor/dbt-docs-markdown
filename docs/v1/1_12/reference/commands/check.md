# About dbt check command

Available in v2

`dbt check` parses your project, runs [checks](../../docs/build/checks.md), and reports results.

## Usage

```shell
dbt check [<check-name> …] [flags]
```

Run all enabled checks:

```shell
dbt check
```

Run one or more checks:

```shell
dbt check all_models_have_descriptions
dbt check all_models_have_descriptions public_models_have_owners
```

Passing an unknown check name fails the command. Passing a disabled check name is accepted and the check is skipped.

## Related docs

* [Checks](../../docs/build/checks.md)
* [`dbt build`](./build.md)
* [check-paths project config](../project-configs/check-paths.md)
