# Setting up dbt State [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

This page walks you through setting up dbt State across dbt Core, dbt platform, and Fusion.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before you set up dbt State, make sure you have:

* **A supported dbt version**: dbt State is natively available in dbt platform and the dbt Fusion engine. It's also available as a plugin for dbt Core v1.7–1.12.
* **A supported data platform**: Snowflake, Databricks, BigQuery, or Redshift. More warehouses are on the roadmap.
* **A dbt State account**: Authenticate through a dbt platform account or a [standalone dbt State account](https://app.state.dbt.com). Refer to [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md#signing-up-for-dbt-state) to choose the right option, and [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing/dbt-state-usage.md) for pricing details. Note that dbt State isn't available on [legacy Starter](https://docs.getdbt.com/docs/platform/billing/plans-and-billing.md#legacy-plans) plan. Please [contact dbt Labs](https://www.getdbt.com/contact) if that applies to you.

## Setting up dbt State[​](#setting-up-dbt-state "Direct link to Setting up dbt State")

Set up dbt State either in dbt platform or locally in dbt Core by using the following steps depending on how you're using dbt.

* dbt platform
* Fusion
* dbt Core 1.7–1.12

#### Enabling dbt State on your account[​](#enabling-dbt-state-on-your-account "Direct link to Enabling dbt State on your account")

**Prerequisite**: You must be an admin in your dbt platform account.

To enable dbt State:

1. In your dbt platform account, click your account name in the lower-left corner above your username and click **Account settings**.

2. Under **Settings**, go to **Billing & Usage** > **Usage-based features**.

3. Under the **State** tab, click **Start free trial**.

   Once started, you cannot pause the trial. After 30 days, you must add a credit card or enterprise contract to continue. For information about how the trial period and billing work, refer to [dbt State trial and billing](https://docs.getdbt.com/docs/deploy/dbt-state-trial.md).

   Extended trial for state-aware orchestration users

   If you're using state-aware orchestration prior to June 1, 2026, your dbt State trial will be extended until the billing period begins on September 1, 2026. If the extension isn’t applied to your account, contact your account team.

4. Review and agree to the terms of service.

5. Click **Start 30-day trial**.

6. Click **Enable dbt State**.

7. Select the jobs to enable dbt State for. You can either enable:

   * **By environment**: Enables dbt State on all existing jobs within the selected environment at once. New deploy jobs created in that environment will have dbt State enabled automatically.
   * **By specific jobs**: Enables dbt State on individual jobs. To enable it on additional jobs later, refer to [Enabling dbt State on individual jobs](https://docs.getdbt.com/docs/deploy/dbt-state-enable-jobs.md).

8. Click **Enable dbt State**.

For next steps, see:

* [Enable dbt State on individual jobs](https://docs.getdbt.com/docs/deploy/dbt-state-enable-jobs.md)
* [Enable dbt State in Studio](https://docs.getdbt.com/docs/deploy/dbt-state-enable-studio.md)

1. Navigate to your project:

   ```bash
   cd to/your/project
   ```

2. Log in to dbt State:

   ```bash
   dbt login
   ```

   This opens a browser window where you can log in with your dbt platform account or the [standalone dbt State app](https://app.state.dbt.com). For details on authentication behavior and how it affects [`user_settings.yml`](https://docs.getdbt.com/reference/global-configs/user-settings.md), refer to [`dbt login` with dbt State](https://docs.getdbt.com/reference/commands/login.md#dbt-login-with-dbt-state).

dbt State is now enabled and will run automatically on every `dbt run` or `dbt build`.

You can also enable or disable dbt State per run using [CLI flags](https://docs.getdbt.com/reference/global-configs/about-global-configs.md): `--manage-state` or `--no-manage-state`, or set the `DBT_ENGINE_MANAGE_STATE` environment variable.

To enable dbt State for everyone on your project, add [`manage_state: true`](https://docs.getdbt.com/reference/global-configs/about-global-configs.md) to the `flags:` block in `dbt_project.yml`:

```yaml
flags:
  manage_state: true
```

dbt State is available as a plugin for dbt Core v1.7+. If you are running on dbt Core v1.9 or older, we encourage you to upgrade to a [more recent version with ongoing support](https://docs.getdbt.com/docs/dbt-versions.md#latest-releases).

To install the plugin:

1. Navigate to your project:

   ```bash
   cd to/your/project
   ```

2. Create and activate a virtual environment:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Install the dbt State plugin:

   ```bash
   pip install dbt-state
   ```

dbt State is now enabled. The first time you execute `dbt run` or `dbt build`, a browser window opens where you can log in with your dbt platform account or the [standalone dbt State app](https://app.state.dbt.com). After authenticating, dbt State runs automatically on every `dbt run` or `dbt build`.

The CLI flags `--manage-state` and `--no-manage-state` are not available in older dbt Core versions. Use the environment variable (`DBT_ENGINE_ENABLE_STATE`) or project flag (`enable_state`) to enable or disable dbt State.

To see how dbt State optimizes your runs, refer to [dbt State usage examples](https://docs.getdbt.com/docs/deploy/dbt-state-examples.md).

## Configuring lag tolerance[​](#configuring-lag-tolerance "Direct link to Configuring lag tolerance")

Lag tolerance allows you to set a tolerance level for older data at the project, environment, or model level. If not configured, `lag_tolerance` defaults to `45m`. We recommend starting with the following Jinja expression:

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: "{{ '4h' if target.name == 'prod' else '7d' }}"
```

In this example, models in the `prod` target rebuild only when upstream data is more than 4 hours old. In all other environments, models wait 7 days before rebuilding.

For more details, refer to the [`lag_tolerance` config reference](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md).

## Inviting team members[​](#inviting-team-members "Direct link to Inviting team members")

The more team members you have using dbt State, the better it gets; more team members means more opportunities to clone existing nodes rather than rebuilding them.

* **For [standalone app](https://app.state.dbt.com) users**: Click the invite link in the upper-right corner of the **Users** page.
* **For dbt platform users**: Have your colleagues run [`dbt login`](https://docs.getdbt.com/reference/commands/login.md) after dbt State is enabled on the account.

## Debugging dbt State[​](#debugging-dbt-state "Direct link to Debugging dbt State")

If dbt State is behaving unexpectedly, you can prepend your run command with the `DBT_ENGINE_MANAGE_STATE` environment variable to isolate the issue:

```bash
DBT_ENGINE_MANAGE_STATE=0 dbt run --target dev --select "customers"
```

## Next steps[​](#next-steps "Direct link to Next steps")

* [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md)
* [`dbt login` with dbt State](https://docs.getdbt.com/reference/commands/login.md#dbt-login-with-dbt-state)
* [Configure deferral](https://docs.getdbt.com/docs/deploy/dbt-state-deferral.md)
* [Non-interactive environment setup](https://docs.getdbt.com/docs/deploy/dbt-state-cicd.md)
* [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md)
