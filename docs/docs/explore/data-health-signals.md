# Data health signals [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Data health signals offer a quick, at-a-glance view of data health when browsing your resources in Catalog. They keep you informed on the status of your resource's health using the indicators **Healthy**, **Caution**, **Degraded**, or **Unknown**.

Note, we don’t calculate data health for non-dbt resources.

* Supported resources are [models](https://docs.getdbt.com/docs/build/models.md), [sources](https://docs.getdbt.com/docs/build/sources.md), and [exposures](https://docs.getdbt.com/docs/build/exposures.md).
* For accurate health data, ensure the resource is up-to-date and had a recent job run.
* Each data health signal reflects key data health components, such as test success status, missing resource descriptions, missing tests, absence of builds in 30-day windows, [and more](#data-health-signal-criteria).

[![View data health signals for your models.](/img/docs/collaborate/dbt-explorer/data-health-signal.jpg?v=2 "View data health signals for your models.")](#)View data health signals for your models.

## Access data health signals[​](#access-data-health-signals "Direct link to Access data health signals")

Access data health signals in the following places:

* In the [search function](https://docs.getdbt.com/docs/explore/explore-projects.md#search-resources) or under **Models**, **Sources**, or **Exposures** in the **Resource** tab.
  <!-- -->
  * For sources, the data health signal also indicates the [source freshness](https://docs.getdbt.com/docs/deploy/source-freshness.md) status.
* In the **Health** column on [each resource's details page](https://docs.getdbt.com/docs/explore/explore-projects.md#view-resource-details). Hover over or click the signal to view detailed information.
* In the **Health** column of public models tables.
* In the [DAG lineage graph](https://docs.getdbt.com/docs/explore/explore-projects.md#project-lineage). Click any node to open the node details panel where you can view it and its details.
* In [Data health tiles](https://docs.getdbt.com/docs/explore/data-tile.md) through an embeddable iFrame and visible in your BI dashboard.

[![Access data health signals in multiple places in dbt Catalog.](/img/docs/collaborate/dbt-explorer/data-health-signal.gif?v=2 "Access data health signals in multiple places in dbt Catalog.")](#)Access data health signals in multiple places in dbt Catalog.

## Data health signal criteria[​](#data-health-signal-criteria "Direct link to Data health signal criteria")

Each resource has a health state that is determined by specific set of criteria. Select the following tabs to view the criteria for that resource type.

* Models
* Sources
* Exposures

The health state of a model is determined by the following criteria:

| **Health state** | **Criteria**                                                                                                                                                                                                                                                                                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ✅ **Healthy**   | All of the following must be true:<br /><br />- Built successfully in the last run<br />- Built in the last 30 days<br />- Model has tests configured<br />- All tests passed<br />- All upstream [sources are fresh](https://docs.getdbt.com/docs/build/sources.md#source-data-freshness) or freshness is not applicable (set to `null`)<br />- Has a description  |
| 🟡 **Caution**   | One of the following must be true:<br /><br />- Not built in the last 30 days<br />- Tests are not configured<br />- Tests return warnings<br />- One or more upstream sources are stale:<br />    - Has a freshness check configured<br />    - Freshness check ran in the past 30 days<br />    - Freshness check returned a warning<br />- Missing a description |
| 🔴 **Degraded**  | One of the following must be true:<br /><br />- Model failed to build<br />- Model has failing tests<br />- One or more upstream sources are stale:<br />    - Freshness check hasn’t run in the past 30 days<br />    - Freshness check returned an error                                                                                                          |
| ⚪ **Unknown**   | - Unable to determine health of resource; no job runs have processed the resource.                                                                                                                                                                                                                                                                                  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The health state of a source is determined by the following criteria:

| **Health state** | **Criteria**                                                                                                                                                                                             |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ✅ Healthy       | All of the following must be true:<br /><br />- Freshness check configured<br />- Freshness check passed<br />- Freshness check ran in the past 30 days<br />- Has a description                         |
| 🟡 Caution       | One of the following must be true:<br /><br />- Freshness check returned a warning<br />- Freshness check not configured<br />- Freshness check not run in the past 30 days<br />- Missing a description |
| 🔴 Degraded      | - Freshness check returned an error                                                                                                                                                                      |
| ⚪ Unknown       | Unable to determine health of resource; no job runs have processed the resource.                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The health state of an exposure is determined by the following criteria:

| **Health state** | **Criteria**                                                                                                                                                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ✅ Healthy       | All of the following must be true:<br /><br />- Underlying sources are fresh<br />- Underlying models built successfully<br />- Underlying models’ tests passing<br />                                                                      |
| 🟡 Caution       | One of the following must be true:<br /><br />- At least one underlying source’s freshness checks returned a warning<br />- At least one underlying model was skipped<br />- At least one underlying model’s tests returned a warning<br /> |
| 🔴 Degraded      | One of the following must be true:<br /><br />- At least one underlying source’s freshness checks returned an error<br />- At least one underlying model did not build successfully<br />- At least one model’s tests returned an error     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
