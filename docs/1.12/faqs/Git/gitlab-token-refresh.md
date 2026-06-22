# GitLab token refresh message

When you connect dbt to a GitLab repository, GitLab automatically creates a [project access token](https://docs.gitlab.com/ee/user/project/settings/project_access_tokens.html) in your GitLab repository in the background. This sends the job run status back to Gitlab using the dbt API for CI jobs.

By default, the project access token follows a naming pattern: `dbt token for GitLab project: <project_id>`. If you have multiple tokens in your repository, look for one that follows this pattern to identify the correct token used by dbt.

If you're receiving a "Refresh token" message, don't worry — dbt automatically refreshes this project access token for you, which means you never have to manually rotate it.

If you still experience any token refresh errors, please try disconnecting and reconnecting the repository in your dbt project to refresh the token.

For any issues, please reach out to the Support team at <support@getdbt.com> and we'll be happy to help!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
