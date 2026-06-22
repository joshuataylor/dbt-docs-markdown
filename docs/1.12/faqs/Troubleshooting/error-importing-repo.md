# Errors importing a repository on dbt project set up

If you don't see your repository listed, double-check that:

* Your repository is in a Gitlab group you have access to. dbt will not read repos associated with a user.

If you do see your repository listed, but are unable to import the repository successfully, double-check that:

* You are a maintainer of that repository. Only users with maintainer permissions can set up repository connections.

If you imported a repository using the dbt native integration with GitLab, you should be able to see if the clone strategy is using a `deploy_token`. If it's relying on an SSH key, this means the repository was not set up using the native GitLab integration, but rather using the generic git clone option. The repository must be reconnected in order to get the benefits described above.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
