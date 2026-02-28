# How to delete a job or environment in dbt?

To delete an environment or job in dbt, you must have a `developer` [license](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md) and have the necessary [access permissions](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md).

### Delete a job[​](#delete-a-job "Direct link to Delete a job")

<!-- -->

To delete a job or multiple jobs in dbt:

1. Click **Deploy** on the navigation header.
2. Click **Jobs** and select the job you want to delete.
3. Click **Settings** on the top right of the page and then click **Edit**.
4. Scroll to the bottom of the page and click **Delete job** to delete the job.
   <br />

[![Delete a job](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/delete-job.png?v=2 "Delete a job")](#)Delete a job

5. Confirm your action in the pop-up by clicking **Confirm delete** in the bottom right to delete the job immediately. This action cannot be undone. However, you can create a new job with the same information if the deletion was made in error.
6. Refresh the page, and the deleted job should now be gone. If you want to delete multiple jobs, you'll need to perform these steps for each job.

If you're having any issues, feel free to [contact us](mailto:support@getdbt.com) for additional help.

### Delete an environment[​](#delete-an-environment "Direct link to Delete an environment")

<!-- -->

Deleting an environment automatically deletes its associated job(s). If you want to keep those jobs, move them to a different environment first.

Follow these steps to delete an environment in dbt:

1. Click **Deploy** on the navigation header and then click **Environments**
2. Select the environment you want to delete.
3. Click **Settings** on the top right of the page and then click **Edit**.
4. Scroll to the bottom of the page and click **Delete** to delete the environment.

[![Delete an environment](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/delete-environment.png?v=2 "Delete an environment")](#)Delete an environment

5. Confirm your action in the pop-up by clicking **Confirm delete** in the bottom right to delete the environment immediately. This action cannot be undone. However, you can create a new environment with the same information if the deletion was made in error.
6. Refresh your page and the deleted environment should now be gone. To delete multiple environments, you'll need to perform these steps to delete each one.

If you're having any issues, feel free to [contact us](mailto:support@getdbt.com) for additional help.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
