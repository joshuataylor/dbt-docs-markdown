# latest\_version

models/\<schema>.yml

```yml
models:
  - name: model_name
    latest_version: 2
    versions:
      - v: 2
      - v: 1
```

## Definition[​](#definition "Direct link to Definition")

The latest version of this model. The "latest" version is relevant for:

1. Resolving `ref()` calls to this model that are "unpinned" (a version is not explicitly specified)
2. Selecting model versions using the [`version:` selection method](https://docs.getdbt.com/reference/node-selection/methods.md#version), based on whether a given model version is `latest`, `prerelease`, or `old`

This value can be a string or a numeric (integer or float) value. It must be one of the [version identifiers](https://docs.getdbt.com/reference/resource-properties/versions.md#v) specified in this model's list of `versions`.

To run the latest version of a model, you can use the [`--select` flag](https://docs.getdbt.com/reference/node-selection/syntax.md). Refer to [Model versions](https://docs.getdbt.com/docs/mesh/govern/model-versions.md#run-a-model-with-multiple-versions) for more information and syntax.

## Default[​](#default "Direct link to Default")

If not specified for a versioned model, `latest_version` defaults to the largest [version identifier](https://docs.getdbt.com/reference/resource-properties/versions.md#v): numerically greatest (if all version identifiers are numeric), otherwise the alphabetically last (if they are strings).

For a non-versioned model (no `versions` list), `latest_version` has no value.

If `latest_version` is not specified for a versioned model, `latest_version` defaults to the largest.

## Example[​](#example "Direct link to Example")

models/\<schema>.yml

```yml
models:
  - name: model_name
    versions:
      - v: 3
      - v: 2
      - v: 1
```

If `latest_version` is not specified, the `latest_version` is `3`. Any unpinned references -- `ref('model_name')` -- will resolve to `model_name.v3`. Both `v1` and `v2` are considered "old" versions.

models/\<schema>.yml

```yml
models:
  - name: model_name
    latest_version: 2
    versions:
      - v: 3
      - v: 2
      - v: 1
```

In this case, the `latest_version` is explicitly set to `2`. Any unpinned references will resolve to `model_name.v2`. `v3` is considered "prerelease", and `v1` is considered "old".

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
