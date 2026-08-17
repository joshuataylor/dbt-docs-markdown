# User settings

`~/.dbt/user_settings.yml` is a user-scoped configuration file for personal dbt preferences. Because it always lives at `~/.dbt/`, it applies globally across all projects on your machine, regardless of which project you're running or where your `profiles.yml` is located.

Today, `user_settings.yml` is primarily written by [`dbt login`](../commands/login.md?version=2.0#dbt-login-with-dbt-state) to configure [dbt State](../../docs/deploy/dbt-state-about.md) locally (`manage_state`). dbt may add other user-scoped flags to this file over time.

Settings in this file are separate from project-level configuration ([`dbt_project.yml`](../dbt_project.yml.md)) and connection configuration ([`profiles.yml`](../../docs/local/profiles.yml.md)). Use this file for preferences that belong to you, not the project, as an alternative to setting environment variables or populating an `.env` file.

\~/.dbt/user\_settings.yml

```yaml
flags:
  manage_state: true
```

Some dbt commands write to this file automatically. You can also edit it manually.

When you run [`dbt login`](../commands/login.md?version=2.0#dbt-login-with-dbt-state), both authentication paths write to `user_settings.yml`:

* **Log in with your dbt platform account**

  * In the dbt Fusion engine, the CLI prompts you before writing to `user_settings.yml`.

  - Setting `manage_state: true` enables dbt State locally on every `dbt run` or `dbt build`.

* **Log in with the standalone dbt State app**: After you create an account, dbt automatically enables dbt State locally in `user_settings.yml`.

If `user_settings.yml` already contains a value, [`dbt login`](../commands/login.md?version=2.0#dbt-login-with-dbt-state) prompts you before overwriting it.

## Config precedence

`user_settings.yml` is the lowest precedence level in dbt's flag hierarchy. More specific settings always override it. For more on flag precedence, refer to [About flags (global configs)](./about-global-configs.md).

1. CLI option (highest)
2. Environment variable
3. Project scope (`dbt_project.yml`)
4. User scope (`~/.dbt/user_settings.yml`) (lowest)

For example, if `manage_state: true` is set in `user_settings.yml` but you run `dbt run --no-manage-state`, the CLI flag takes precedence, and dbt disables dbt State for that invocation.

## When dbt State is enabled locally but not in dbt platform

If `manage_state: true` is set in `user_settings.yml` but dbt State is not enabled in your dbt platform account, dbt fails with an error on your next `dbt run` or `dbt build`. To resolve this, either:

* [Enable dbt State in your dbt platform account](../../docs/deploy/dbt-state-setup.md).
* Disable dbt State locally by setting `manage_state: false` in `user_settings.yml`, or run `dbt run --no-manage-state`.
