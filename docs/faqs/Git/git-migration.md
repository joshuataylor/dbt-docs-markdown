# How to migrate git providers

To migrate from one git provider to another, refer to the following steps to avoid minimal disruption:

1. Outside of dbt, you'll need to import your existing repository into your new provider. By default, connecting your repository in one account won't automatically disconnected it from another account.

   As an example, if you're migrating from GitHub to Azure DevOps, you'll need to import your existing repository (GitHub) into your new Git provider (Azure DevOps). For detailed steps on how to do this, refer to your Git provider's documentation (Such as [GitHub](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer), [GitLab](https://docs.gitlab.com/ee/user/project/import/repo_by_url.html), [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/repos/git/import-git-repository?view=azure-devops))

2. Go back to dbt and set up your [integration for the new Git provider](https://docs.getdbt.com/docs/cloud/git/git-configuration-in-dbt-cloud.md), if needed.

3. Disconnect the old repository in dbt by going to **Account Settings** and then **Projects**.

4. Click on the **Repository** link, then click **Edit** and **Disconnect**.

   [![Disconnect and reconnect your Git repository in your dbt Account settings page.](/img/docs/dbt-cloud/disconnect-repo.png?v=2 "Disconnect and reconnect your Git repository in your dbt Account settings page.")](#)Disconnect and reconnect your Git repository in your dbt Account settings page.

5. Click **Confirm Disconnect**.

6. On the same page, connect to the new Git provider repository by clicking **Configure Repository**

   * If you're using the native integration, you may need to OAuth to it.

7. That's it, you should now be connected to the new Git provider! 🎉

Note — As a tip, we recommend you refresh your page and Studio IDE before performing any actions.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
