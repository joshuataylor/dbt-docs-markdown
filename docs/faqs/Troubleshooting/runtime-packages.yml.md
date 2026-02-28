# Why am I receiving a Runtime Error in my packages?

If you're receiving the runtime error below in your packages.yml folder, it may be due to an old version of your dbt\_utils package that isn't compatible with your current dbt version.

```shell
Running with dbt=xxx
Runtime Error
  Failed to read package: Runtime Error
    Invalid config version: 1, expected 2  
  Error encountered in dbt_utils/dbt_project.yml
```

Try updating the old version of the dbt\_utils package in your packages.yml to the latest version found in the [dbt hub](https://hub.getdbt.com/dbt-labs/dbt_utils/latest/):

```shell
packages:
- package: dbt-labs/dbt_utils

version: xxx
```

If you've tried the workaround above and are still experiencing this behavior - reach out to the Support team at <support@getdbt.com> and we'll be happy to help!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
