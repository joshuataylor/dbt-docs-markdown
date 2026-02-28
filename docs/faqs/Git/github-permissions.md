# I'm seeing a 'GitHub and dbt latest permissions' error

If you see the error `This account needs to accept the latest permissions for the dbt GitHub App` in dbt — this usually occurs when the permissions for the dbt GitHub App are out-of-date.

To solve this issue, you'll need to update the permissions for the dbt GitHub App in your GitHub account. This FAQ shares a couple of ways you can do it.

## Update permissions[​](#update-permissions "Direct link to Update permissions")

A GitHub organization admin will need to update the permissions in GitHub for the dbt GitHub App. If you're not the admin, reach out to your organization admin to request this.

1. Navigate to your GitHub account. Click on the top right profile icon and then **Settings** (or personal if using a non-organization account).

2. Then go to **Integrations** and then select **Applications** to identify any necessary permission changes. Note that a GitHub repository admin may not see the same permission request.

[![Navigate to Application settings to identify permission changes.](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/github-applications.png?v=2 "Navigate to Application settings to identify permission changes.")](#)Navigate to Application settings to identify permission changes.

3. Click on **Review request** and then click on the **Accept new permissions** button on the next page.

[![Grant access to the dbt app by accepting the new permissions.](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/github-review-request.png?v=2 "Grant access to the dbt app by accepting the new permissions.")](#)Grant access to the dbt app by accepting the new permissions.

For more info on GitHub permissions, refer to [access permissions](https://docs.github.com/en/get-started/learning-about-github/access-permissions-on-github).

Alternatively, try [disconnecting your GitHub account](#disconnect-github) in dbt, detailed in the following section.

## Disconnect GitHub[​](#disconnect-github "Direct link to Disconnect GitHub")

Disconnect the GitHub and dbt integration in dbt.

1. In dbt, go to **Account Settings**.
2. In **Projects**, select the project experiencing the issue.
3. Click the repository link under **Repository**.
4. In the **Repository details** page, click **Edit**.
5. Click **Disconnect** to remove the GitHub integration.
   <!-- -->
   [![Disconnect and reconnect your git repository in your dbt Account settings pages.](/img/docs/dbt-cloud/disconnect-repo.png?v=2 "Disconnect and reconnect your git repository in your dbt Account settings pages.")](#)Disconnect and reconnect your git repository in your dbt Account settings pages.
6. Click **Confirm Disconnect**.
7. Return to your **Project details** page and reconnect your repository by clicking the **Configure Repository** link.
8. Click **GitHub** and select your repository.

## Support[​](#support "Direct link to Support")

If you've tried these workarounds and are still experiencing this behavior — reach out to the [dbt Support](mailto:support@getdbt.com) team and we'll be happy to help!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
