# name

dbt\_project.yml

```yml
name: string
```

## Definition[​](#definition "Direct link to Definition")

**Required configuration**

The name of a dbt project. Must be letters, digits and underscores only, and cannot start with a digit.

## Recommendation[​](#recommendation "Direct link to Recommendation")

Often an organization has one dbt project, so it is sensible to name a project with your organization's name, in `snake_case`. For example:

* `name: acme`
* `name: jaffle_shop`
* `name: evilcorp`

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

### Invalid project name[​](#invalid-project-name "Direct link to Invalid project name")

```text
Encountered an error while reading the project:
  ERROR: Runtime Error
  at path ['name']: 'jaffle-shop' does not match '^[^\\d\\W]\\w*$'
Runtime Error
  Could not run dbt
```

This project has:

dbt\_project.yml

```yml
name: jaffle-shop
```

In this case, change your project name to be `snake_case` instead:

dbt\_project.yml

```yml
name: jaffle_shop
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
