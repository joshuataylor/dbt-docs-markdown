# Can I define private packages in the dependencies.yml file?

It depends on how you're accessing your private packages:

* If you're using [native private packages](https://docs.getdbt.com/docs/build/packages.md#native-private-packages), you can define them in the `dependencies.yml` file.
* If you're using the [git token method](https://docs.getdbt.com/docs/build/packages.md#git-token-method), you must define them in the `packages.yml` file instead of the `dependencies.yml` file. This is because conditional rendering (like Jinja-in-yaml) is not supported in `dependencies.yml`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
