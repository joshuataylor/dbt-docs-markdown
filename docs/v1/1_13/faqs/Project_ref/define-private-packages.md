# Can I define private packages in the dependencies.yml file?

It depends on how you're accessing your private packages:

* If you're using [native private packages](../../docs/build/packages.md#native-private-packages), you can define them in the `dependencies.yml` file.
* If you're using the [git token method](../../docs/build/packages.md#git-token-method), use `packages.yml` on dbt v1 because Jinja isn't supported in `dependencies.yml`. On dbt v2, you can use `dependencies.yml`, but we recommend using `packages.yml` if your project must also run on dbt v1. Refer to [Jinja support by file type](../../reference/jinja-file-support.md).
