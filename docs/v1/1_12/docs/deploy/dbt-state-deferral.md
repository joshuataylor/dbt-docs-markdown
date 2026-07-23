# Configuring deferral in dbt State [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

By default, dbt State defers to your production environment. Both sections on this page are optional; configure them if you want to customize the defaults.

## Configure deferral[​](#configure-deferral "Direct link to Configure deferral")

By default, dbt State defers to your production environment. You only need to configure this if you want to change that behavior:

* **dbt platform**: To defer to an environment other than the default (for example, staging), add `defer-env-id` to the `dbt-cloud` block in `dbt_project.yml`. Refer to [Configure Cloud CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) for more information.

  dbt\_project.yml

  ```yaml
  dbt-cloud:
    project-id: <your-project-id>
    defer-env-id: <your-environment-id>
  ```

* **Self-managed deployments**: If you can't access a production or deployment manifest, you can set [`defer_to_target`](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md) in `profiles.yml` for best-effort auto-deferral. Note that this approach has known limitations; refer to [Caveats to dbt State without a manifest](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md#caveats-to-dbt-state-without-a-manifest).

  profiles.yml

  ```yaml
  my_project:
    outputs:
      uat:
        type: snowflake
        # ... connection settings
        defer_to_target: staging
  ```

You can also pass `--state` or `--defer-state` to explicitly point dbt State to a specific `manifest.json`.

note

If you've overridden `generate_*_name()` macros with runtime values (such as environment variables, file paths, or dates), provide a `manifest.json` file so dbt State can locate objects correctly. Without one, it infers object locations from your macros and profile target, which may be incorrect in these cases. Refer to [Caveats to dbt State without a manifest](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md#caveats-to-dbt-state-without-a-manifest).

## Specify your project or org[​](#specify-your-project-or-org "Direct link to Specify your project or org")

If you have multiple projects or orgs that use dbt State, configure the `dbt-cloud` block in `dbt_project.yml` so dbt State knows which one to use:

* **For dbt platform users with multiple projects**: Add `project-id` to identify which project dbt State should use.

  dbt\_project.yml

  ```yaml
  dbt-cloud:
    project-id: <your-project-id>
  ```

* **For self-managed deployments with multiple dbt State orgs**: Add `state-org-id` to identify which org dbt State should use.

  dbt\_project.yml

  ```yaml
  dbt-cloud:
    state-org-id: <your-org-id>
  ```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
* [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md)
