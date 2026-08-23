# Migrate off legacy dbt versions

[Back to guides](../guides.md)



Legacy dbt Core versions v1.3–v1.7 are being deprecated on January 31, 2027. To keep your work running and supported, move your environments and jobs to a [dbt Core release track](../docs/dbt-versions/dbt-release-tracks.md) now. A release track keeps you on a supported version automatically and prepares your project for [dbt Fusion engine](../docs/fusion/about-fusion.md) later.

The single-hop path

Move to a dbt Core release track now; move to Fusion later. Migrating first to a supported release track lowers your migration risk and gets you on a supported version faster.

Your project code, connections, and history stay accessible throughout.

## Identify projects using legacy versions

What you migrate is driven by a job's effective version: the version pinned on the job if one is set, otherwise the version its environment inherits.

1. Navigate to **Orchestration** > **Environments** and note any environment whose **dbt version** is v1.3–v1.7. The current version is displayed below the environment name.
2. Open the jobs in your supported environments and check for any job with a **version override** pinned to v1.3–v1.7.

## Select your path

Find the row that matches each environment, then follow the linked steps.

| Environment version                        | Job version                                              | What you do                                                                                                               |
| ------------------------------------------ | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Legacy (v1.3–v1.7)                         | Inherits from environment, or pinned to a legacy version | [Migrate the environment to a release track](#migrate-an-environment-to-a-release-track)                                  |
| Legacy (v1.3–v1.7)                         | At least one job pinned to a supported version           | [Migrate the environment to a release track](#migrate-an-environment-to-a-release-track); the supported jobs are retained |
| Supported                                  | One or more jobs pinned to a legacy version              | [Update the job's version](#update-a-jobs-version); the environment is untouched                                          |
| Legacy and **dormant** (unused 12+ months) | —                                                        | [Delete the environment](#delete-a-dormant-environment), or migrate it if you still need it                               |

If you don't migrate a legacy environment or clear a legacy job, it's cleaned up when legacy versions are retired: a legacy environment (and its jobs) is deleted, a legacy environment that already has a job on a supported version is **updated** with only the legacy jobs removed, and a supported environment with legacy-pinned jobs keeps the environment and loses **only those jobs**.

## Migrate an environment to a release track

To update your environment to a release track:

1. Navigate to the Settings page of the environment, then click **Edit**.
2. Click the **dbt version** dropdown and select a [release track](../docs/dbt-versions/dbt-release-tracks.md) (**Latest** is recommended).
3. Save your changes.

As a best practice, test the upgrade in a development environment first. See [Upgrade versions in dbt platform](../docs/dbt-versions/upgrade-dbt-platform-version.md) for details, including how to set the version through the [Admin API](../docs/dbt-apis/admin-api.md) or Terraform.

## Update a job's version

If a job on a supported environment is pinned to a legacy version, clear or change the override:

1. Open the job's settings and find the **dbt version** setting.
2. Either clear the override so the job inherits the environment's version, or set it to a [release track](../docs/dbt-versions/dbt-release-tracks.md).
3. Save your changes.

## Delete a dormant environment

If a legacy environment has been dormant for a year or more, and you no longer need it, delete it. (If you still need it, [migrate it to a release track](#migrate-an-environment-to-a-release-track) instead.)

Deleting an environment automatically deletes its associated job(s). If you want to keep those jobs, move them to a different environment first.

Follow these steps to delete an environment in dbt:

1. Navigate to **Orchestration** > **Environments**.
2. Select the environment you want to delete.
3. Click **Settings** on the top right of the page and then click **Edit**.
4. Scroll to the bottom of the page and click **Delete** to delete the environment.

[![Delete an environment](/img/docs/dbt-platform/platform-configuring-dbt-platform/delete-environment.png?v=2 "Delete an environment")](#)Delete an environment

5. Confirm your action in the pop-up by clicking **Confirm delete** in the bottom right to delete the environment immediately. This action cannot be undone. However, you can create a new environment with the same information if the deletion was made in error.
6. Refresh your page and the deleted environment should now be gone. To delete multiple environments, you'll need to perform these steps to delete each one.

If you're having any issues, feel free to [contact us](mailto:support@getdbt.com) for additional help.

## Validate your migration

Moving from a pinned legacy version to a release track can surface behavior changes, dependency issues, or adapter differences that weren't visible before. To reduce risk:

* Test in a development environment before upgrading your production and default development environments.
* Review your compile, build, and job run results after the change.
* Establish a fallback path in case you need to roll back.

## Get help

If you hit a blocker you can't resolve, [contact Support](mailto:support@getdbt.com) with your project ID, environment ID, affected job run IDs and logs, your current version, and your target release track.
