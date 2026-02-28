# Connect with managed repository

Managed repositories are a great way to trial dbt without needing to create a new repository. If you don't already have a Git repository for your dbt project, you can let dbt host and manage a repository for you.

If in the future you choose to host this repository elsewhere, you can export the information from dbt at any time. Refer to [Move from a managed repository to a self-hosted repository](https://docs.getdbt.com/faqs/Git/managed-repo.md) for more information on how to do that.

info

dbt Labs recommends against using a managed repository in a production environment. You can't use Git features like pull requests, which are part of our recommended version control best practices.

To set up a project with a managed repository:

1. From your **Account settings** in dbt, select the project you want to set up with a managed repository. If the project already has a repository set up, you need to edit the repository settings and disconnect the existing repository.
2. Click **Edit** for the project.
3. Under Repository, click **Configure repository**.
4. Select **Managed**.
5. Enter a name for the repository. For example, "analytics" or "dbt-models."
6. Click **Create**.
   <!-- -->
   [![Adding a managed repository](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/managed-repo.png?v=2 "Adding a managed repository")](#)Adding a managed repository

## Download managed repository[​](#download-managed-repository "Direct link to Download managed repository")

To download a copy of your managed repository from dbt to your local machine:

1. Use the **Project** selector on the main left-side menu to navigate to a project that's using a managed repository.
2. Click **Dashboard** from the main left-side menu.
3. From the dashboard, click **Settings**.
4. Locate the **Repository** field and click the hyperlink for the repo.
5. Below the **Deploy key** you will find the **Download repository** option. Click the button to download. If you don't see this option, you're either not assigned a [permission set](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md#account-permissions) with `write` access to Git repositories, or you don't have a managed repo for your project.

[![The download button for a managed repo.](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/download-managed-repo.png?v=2 "The download button for a managed repo.")](#)The download button for a managed repo.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
