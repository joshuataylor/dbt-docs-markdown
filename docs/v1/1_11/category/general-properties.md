*
* [Resource configs and properties](../reference/resource-configs/resource-path.md)
* General properties

# General properties

The list of general properties available in dbt.

## [anchors](../reference/resource-properties/anchors.md)

[Use the anchors key to hold reusable YAML config fragments so they pass file validation.](../reference/resource-properties/anchors.md)

## [columns](../reference/resource-properties/columns.md)

[Use the columns property to document and test individual columns on models, sources, seeds, and snapshots.](../reference/resource-properties/columns.md)

## [config](../reference/resource-properties/config.md)

[Use the config property to set configs for a resource directly in its YAML file, instead of in dbt\_project.yml.](../reference/resource-properties/config.md)

## [constraints](../reference/resource-properties/constraints.md)

[Constraints are a feature of many data platforms. When specified, the platform will perform additional validation on data as it is being populated in a new table or inserted into a preexisting table. If the validation fails, the table creation or update fails, the operation is rolled back, and you will see a clear error message.](../reference/resource-properties/constraints.md)

## [deprecation\_date](../reference/resource-properties/deprecation_date.md)

[Set a deprecation\_date on a model to tell consumers when it will stop being supported.](../reference/resource-properties/deprecation_date.md)

## [description](../reference/resource-properties/description.md)

[This guide explains how to use the description key to add YAML descriptions to dbt resources (models, sources, seeds) using markdown and Jinja for better documentation.](../reference/resource-properties/description.md)

## [latest\_version](../reference/resource-properties/latest_version.md)

[Use latest\_version to set which version of a versioned model unpinned ref() calls resolve to.](../reference/resource-properties/latest_version.md)

## [Data tests](../reference/resource-properties/data-tests.md)

[Reference guide for the resource properties available for data tests in dbt.](../reference/resource-properties/data-tests.md)

## [versions](../reference/resource-properties/versions.md)

[Use the versions property to define multiple versions of a model and how each one differs.](../reference/resource-properties/versions.md)

[Previous](../reference/define-properties.md)

[Define properties](../reference/define-properties.md)

[Next](../reference/resource-properties/anchors.md)

[anchors](../reference/resource-properties/anchors.md)
