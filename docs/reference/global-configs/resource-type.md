# Resource type

This means the flags enable you to specify which types of resources to include or exclude when running the commands, instead of targeting specific resources.

Note

The `--exclude-resource-type` flag is only available in dbt version 1.8 and higher. If you're using older versions, this flag won't be available.

The available resource types are:

<!-- -->

<!-- -->

## Positive vs negative filters[​](#positive-vs-negative-filters "Direct link to Positive vs negative filters")

* `--resource-type` is a positive filter — dbt only runs the resource types selected in the command, implicitly skipping every other type.
* `--exclude-resource-type` is a negative filter — dbt starts with the full catalog of resource types and then omits the types selected in the command. dbt runs everything *except* those resource types.

You can use both flags in a command; dbt first applies the positive filter (`--resource-type`) and then removes the types listed in the negative filter (`--exclude-resource-type`). For example:

```text
dbt build --resource-type model test snapshot --exclude-resource-type snapshot
```

Note that the list of dbt resource types is mutually exclusive and collectively exhaustive (MECE). This means that any `--resource-type` selection can also be achieved by excluding the other resource types using `--exclude-resource-type`, and vice versa.

## Examples[​](#examples "Direct link to Examples")

Instead of targeting specific resources, use the `--resource-type` or `--exclude-resource-type` flags to target all resources of a certain type: `dbt build --resource-type RESOURCE_TYPE`, replacing `RESOURCE_TYPE` with the resource type you want to include.

See the following sample commands for including or excluding resource types. Note that the `--exclude-resource-type` flag is only available in dbt version 1.8 and higher.

 Include resource types

### Include multiple resource types[​](#include-multiple-resource-types "Direct link to Include multiple resource types")

Use the following command to include multiple resource types such as data tests and models in your build process:

Usage

```text
dbt build --resource-type test model
```

### Include all snapshots[​](#include-all-snapshots "Direct link to Include all snapshots")

Use the following command to only include snapshots in your dbt build process:

Usage

```text
dbt build --resource-type snapshot
```

### Include all saved queries[​](#include-all-saved-queries "Direct link to Include all saved queries")

Use the following command to only include saved queries with the `--resource-type` flag:

Usage

```text
dbt build --resource-type saved_query
```

### Include all data tests[​](#include-all-data-tests "Direct link to Include all data tests")

Use the following command to only include data tests in your build process:

Usage

```text
dbt build --resource-type test
```

 Exclude resource types

### Exclude multiple resource types[​](#exclude-multiple-resource-types "Direct link to Exclude multiple resource types")

Use the following command to exclude multiple resource types such as data tests and models from your build process:

Usage

```text
dbt build --exclude-resource-type test model
```

### Exclude all unit tests[​](#exclude-all-unit-tests "Direct link to Exclude all unit tests")

Use the following command to exclude unit tests from your dbt build process.

Usage

```text
dbt build --exclude-resource-type unit_test
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
