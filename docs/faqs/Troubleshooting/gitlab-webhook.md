# Unable to trigger a CI job with GitLab

When you connect dbt to a GitLab repository, GitLab automatically registers a webhook in the background, viewable under the repository settings. This webhook is also used to trigger [CI jobs](https://docs.getdbt.com/docs/deploy/ci-jobs.md) when you push to the repository.

If you're unable to trigger a CI job, this usually indicates that the webhook registration is missing or incorrect.

To resolve this issue, navigate to the repository settings in GitLab and view the webhook registrations by navigating to GitLab --> **Settings** --> **Webhooks**.

Some things to check:

* The webhook registration is enabled in GitLab.
* The webhook registration is configured with the correct URL and secret.

If you're still experiencing this issue, reach out to the Support team at <support@getdbt.com> and we'll be happy to help!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
