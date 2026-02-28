# packages-install-path

dbt\_project.yml

```yml
packages-install-path: directorypath
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom directory where [packages](https://docs.getdbt.com/docs/build/packages.md) are installed when you run the `dbt deps` [command](https://docs.getdbt.com/reference/commands/deps.md). Note that this directory is usually git-ignored.

## Default[​](#default "Direct link to Default")

By default, dbt will install packages in the `dbt_packages` directory, i.e. `packages-install-path: dbt_packages`

## Examples[​](#examples "Direct link to Examples")

### Install packages in a subdirectory named `packages` instead of `dbt_packages`[​](#install-packages-in-a-subdirectory-named-packages-instead-of-dbt_packages "Direct link to install-packages-in-a-subdirectory-named-packages-instead-of-dbt_packages")

dbt\_project.yml

```yml
packages-install-path: packages
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
