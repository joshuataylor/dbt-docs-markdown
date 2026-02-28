# PR template

## Configure pull request (PR) template URLs[​](#configure-pull-request-pr-template-urls "Direct link to Configure pull request (PR) template URLs")

When you commit changes to a branch in the Studio IDE, dbt can prompt users to open a new Pull Request for the code changes. To enable this functionality, ensure that a PR Template URL is configured in the **Repository details** page in your **Account Settings**. If this setting is blank, the Studio IDE will prompt users to merge the changes directly into their default branch.

[![Configure a PR template in the 'Repository details' page.](/img/docs/collaborate/repo-details.jpg?v=2 "Configure a PR template in the 'Repository details' page.")](#)Configure a PR template in the 'Repository details' page.

### PR Template URL by git provider[​](#pr-template-url-by-git-provider "Direct link to PR Template URL by git provider")

The PR Template URL setting will be automatically set for most repositories, depending on the connection method.

* If you connect to your repository via in-app integrations with your git provider or the "Git Clone" method via SSH, this URL setting will be auto-populated and editable.
  <!-- -->
  * For AWS CodeCommit, this URL setting isn't auto-populated and must be [manually configured](https://docs.getdbt.com/docs/cloud/git/import-a-project-by-git-url.md#step-5-configure-pull-request-template-urls-optional).
* If you connect via a dbt [Managed repository](https://docs.getdbt.com/docs/cloud/git/managed-repository.md), this URL will not be set, and the Studio IDE will prompt users to merge the changes directly into their default branch.

The PR template URL supports two variables that can be used to build a URL string. These variables, `{{source}}` and `{{destination}}` return branch names based on the state of the configured Environment and active branch open in the IDE. The `{{source}}` variable represents the active development branch, and the `{{destination}}` variable represents the configured base branch for the environment, eg. `master`.

A typical PR build URL looks like:

* Template
* Rendered

```text
https://github.com/dbt-labs/jaffle_shop/compare/{{destination}}..{{source}}
```

```text
https://github.com/dbt-labs/jaffle_shop/compare/master..my-branch
```

## Example templates[​](#example-templates "Direct link to Example templates")

Some common URL templates are provided below, but please note that the exact value may vary depending on your configured git provider.

### GitHub[​](#github "Direct link to GitHub")

```text
https://github.com/<org>/<repo>/compare/{{destination}}..{{source}}
```

If you're using Github Enterprise your template may look something like:

```text
https://git.<mycompany>.com/<org>/<repo>/compare/{{destination}}..{{source}}
```

### GitLab[​](#gitlab "Direct link to GitLab")

```text
https://gitlab.com/<org>/<repo>/-/merge_requests/new?merge_request[source_branch]={{source}}&merge_request[target_branch]={{destination}}
```

### BitBucket[​](#bitbucket "Direct link to BitBucket")

```text
https://bitbucket.org/<org>/<repo>/pull-requests/new?source={{source}}&dest={{destination}}
```

If you're using BitBucket Server or Data Center your template may look something like:

```text
https://<bitbucket-server>/projects/<proj>/repos/<repo>/pull-requests?create&sourceBranch={{source}}&targetBranch={{destination}}
```

### AWS CodeCommit[​](#aws-codecommit "Direct link to AWS CodeCommit")

```text
https://console.aws.amazon.com/codesuite/codecommit/repositories/<repo>/pull-requests/new/refs/heads/{{destination}}/.../refs/heads/{{source}}
```

### Azure DevOps[​](#azure-devops "Direct link to Azure DevOps")

```text
https://dev.azure.com/<org>/<project>/_git/<repo>/pullrequestcreate?sourceRef={{source}}&targetRef={{destination}}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
