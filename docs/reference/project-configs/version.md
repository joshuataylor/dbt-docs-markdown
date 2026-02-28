# version

Model versions, dbt\_project.yml versions, and .yml versions

The word "version" appears in multiple places in docs site and with different meanings:

* [Model versions](https://docs.getdbt.com/docs/mesh/govern/model-versions.md) — A dbt Mesh feature that enables better governance and data model management by allowing you to track changes and updates to models over time.
* [dbt\_project.yml version](https://docs.getdbt.com/reference/project-configs/version.md#dbt_projectyml-versions)(optional) — `dbt_project.yml` version is unrelated to Mesh and refers to the compatibility of the dbt project with a specific version of dbt.
* [.yml property file version](https://docs.getdbt.com/reference/project-configs/version.md#yml-property-file-versions)(optional) — Version numbers within .yml property files inform how dbt parses those YAML files. Unrelated to Mesh.

dbt projects have two distinct types of `version` tags. This field has a different meaning depending on its location.

## `dbt_project.yml` versions[​](#dbt_projectyml-versions "Direct link to dbt_projectyml-versions")

The version tag in a `dbt_project` file represents the version of your dbt project.

Starting in dbt version 1.5, `version` in the `dbt_project.yml` is an *optional parameter*. If used, the version must be in a [semantic version](https://semver.org/) format, such as `1.0.0`. The default value is `None` if not specified. For users on dbt version 1.4 or lower, this tag is required, though it isn't currently used meaningfully by dbt.

For more on Core versions, see [About dbt Core versions](https://docs.getdbt.com/docs/dbt-versions/core.md).

dbt\_project.yml

```yml
version: version
```

## `.yml` property file versions[​](#yml-property-file-versions "Direct link to yml-property-file-versions")

A version tag in a `.yml` property file provides the control tag, which informs how dbt processes property files.

Starting from version 1.5, dbt will no longer require this configuration in your resource `.yml` files. If you want to know more about why this tag was previously required, you can refer to the [FAQs](#faqs). For users on dbt version 1.4 or lower, this tag is required,

For more on property files, see their general [documentation](https://docs.getdbt.com/reference/define-properties.md) on the same page.

* Resource property file with version specified
* Resource property file without version specified

\<any valid filename>.yml

```yml
version: 2  # Only 2 is accepted by dbt versions up to 1.4.latest.

models: 
    ...
```

\<any valid filename>.yml

```yml

models: 
    ...
```

## FAQS[​](#faqs "Direct link to FAQS")

Why do model and source YAML files always start with \`version: 2\`?

Once upon a time, the structure of these `.yml` files was very different (s/o to anyone who was using dbt back then!). Adding `version: 2` allowed us to make this structure more extensible.

From [dbt Core v1.5](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.5.md#quick-hits>), the top-level `version:` key is optional in all resource YAML files. If present, only `version: 2` is supported.

Also starting in v1.5, both the [`config-version: 2`](https://docs.getdbt.com/reference/project-configs/config-version.md) and the top-level `version:` key in the `dbt_project.yml` are optional.

Resource YAML files do not currently require this config. We only support `version: 2` if it's specified. Although we do not expect to update YAML files to `version: 3` soon, having this config will make it easier for us to introduce new structures in the future

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
