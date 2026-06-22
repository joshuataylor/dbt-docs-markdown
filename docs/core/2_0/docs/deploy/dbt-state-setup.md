# Setting up dbt State [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt State is natively available in dbt platform and locally in dbt Core v1.12+ and the dbt Fusion engine. It is also available as a plugin for older versions of dbt Core (1.7-1.11).

Once enabled, dbt State runs automatically on every `dbt run` or `dbt build`.

Before you begin:

* dbt State supports Snowflake, Databricks, BigQuery, and Redshift.
* dbt State requires authentication either through a dbt platform account, or a [standalone account](https://app.state.dbt.com) that's independent of dbt platform. For details on which option is right for you, refer to [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md#signing-up-for-dbt-state). For pricing information, refer to [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage).

Select the option that matches your setup:

* dbt platform
* dbt Core 1.12 / Fusion
* dbt Core 1.7–1.11

#### Enabling dbt State on your account[​](#enabling-dbt-state-on-your-account "Direct link to Enabling dbt State on your account")

**Prerequisite**: You must be an admin in your dbt platform account.

To enable dbt State:

1. In your dbt platform account, click your account name in the lower-left corner above your username and click **Account settings**.

2. Under **Settings**, go to **State**.

3. Click **Start your 30-day free trial**.

   Once started, you cannot pause the trial. After 30 days, you must add a credit card or enterprise contract to continue. For more information, refer to [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage).

   Extended trial for state-aware orchestration users

   If you're using state-aware orchestration prior to June 1, 2026, your dbt State trial will be extended until the billing period begins on September 1, 2026. If the extension isn’t applied to your account, contact your account team.

4. Review and agree to the terms of service.

5. Click **Start 30-day trial**.

6. Click **Enable dbt State**.

   [![dbt State page](/img/docs/dbt-state/dbt_state_enable.png?v=2 "dbt State page")](#)dbt State page

7. In the **Upgrade to dbt State** page, select the jobs to enable dbt State for. You can either enable:

   * **By environment**: Enables dbt State on all existing jobs within the selected environment at once. New deploy jobs created in that environment will have dbt State enabled automatically.
   * **By specific jobs**: Enables dbt State on individual jobs. To enable it on additional jobs later, refer to [Enabling dbt State on individual jobs](#enabling-dbt-state-on-individual-jobs).

8. Click **Enable dbt State**.

The **dbt State** page where you started your trial in step 3 displays how many days remain in your trial period alongside the following monthly data:

* Number of models reused
* Total % build reduction
* Total query run time reduction

#### Enabling dbt State on individual jobs[​](#enabling-dbt-state-on-individual-jobs "Direct link to Enabling dbt State on individual jobs")

To enable dbt State on any job — whether already existing or newly created in an environment that doesn't have dbt State enabled:

1. Go to **Orchestration** > **Jobs**.
2. Select the job you want dbt State enabled for.
3. Click **Settings** > **Edit**.
4. In the **Execution settings** section of the job, select **Enable dbt State**.
5. Click **Save**.

#### Enabling dbt State in Studio[​](#enabling-dbt-state-in-studio "Direct link to Enabling dbt State in Studio")

When you enable dbt State in the Studio IDE, it runs automatically on every `dbt run` or `dbt build` during development — skipping unchanged models and reusing production results so your runs are *faster*.

You can [turn it on for your development environment](#enabling-dbt-state-on-a-development-environment) so it's the default for everyone, or you can [override that setting just for your own account](#overriding-dbt-state-setting-per-user).

**Prerequisite**: An account admin must [enable dbt State](#enabling-dbt-state-on-your-account) before you can use it.

##### Enabling dbt State on a development environment[​](#enabling-dbt-state-on-a-development-environment "Direct link to Enabling dbt State on a development environment")

Enabling dbt State on your development environment turns it on for everyone using the Studio IDE, unless they override it for their own account.

1. Go to **Orchestration** > **Environments** and select your development environment.
2. Click **Settings** > **Edit**.
3. In the **dbt State** section, select **Enable dbt State**.
4. Click **Save**.
5. In the pop-up box, click **Continue** if you want to go ahead with the changes and restart all IDE sessions for this project.

##### Overriding dbt State setting per user[​](#overriding-dbt-state-setting-per-user "Direct link to Overriding dbt State setting per user")

You can override the development environment's dbt State setting for your own account without affecting other users. Because the user-level setting takes precedence over the environment-level setting, you can turn dbt State on for yourself before enabling it for your whole team, or turn it off when it's enabled at the environment level.

1. Click your account name in the lower-left corner and select **Account settings**.

2. Under **Your profile**, go to **Credentials**.

3. Select the project you want to enable dbt State for.

4. Click **Edit** and go to the **User development settings** section.

5. Under **dbt State**, select one of the following options:

   <!-- -->

   * **Enabled**: Enables dbt State for your user regardless of the development environment setting.
   * **Disabled**: Disables dbt State for your user regardless of the development environment setting.
   * **Reset (inherit from development)**: Only appears after you've saved an **Enabled** or **Disabled** override. Clears your override and falls back to the dbt State setting configured on your development environment.

6. Click **Save**.

1) Navigate to your project:

   ```bash
   cd to/your/project
   ```

2) Log in to dbt State:

   ```bash
   dbt login
   ```

   This opens a browser window where you can log in with your dbt platform account or the [standalone dbt State app](https://app.state.dbt.com). For details on authentication behavior and how it affects [`user_settings.yml`](https://docs.getdbt.com/reference/global-configs/user-settings.md), refer to [`dbt login`](https://docs.getdbt.com/reference/commands/login.md#dbt-login-with-dbt-state).

   If dbt State is already enabled on your dbt platform account, you'll be prompted to enable it on your machine. You can change this at any time by editing the settings file located at `~/.dbt/user_settings.yml`. For more information about login behavior, refer to [`dbt login` with dbt State](https://docs.getdbt.com/reference/commands/login.md#dbt-login-with-dbt-state).

dbt State is now enabled and will run automatically on every `dbt run` or `dbt build`.

You can also enable or disable dbt State per run using [CLI flags](https://docs.getdbt.com/reference/global-configs/about-global-configs.md): `--manage-state` or `--no-manage-state`, or set the `DBT_ENGINE_MANAGE_STATE` environment variable.

To enable dbt State for everyone on your project, add [`manage_state: true`](https://docs.getdbt.com/reference/global-configs/about-global-configs.md) to the `flags:` block in `dbt_project.yml`:

```yaml
flags:
  manage_state: true
```

dbt State is available as a plugin for older versions of dbt Core (v1.7+). If you are running on dbt Core v1.9 or older, we encourage you to upgrade to a [more recent version with ongoing support](https://docs.getdbt.com/docs/dbt-versions.md#latest-releases).

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

dbt State works out of the box, but the following steps can help you get more value from it.

* [Configuring lag tolerance](#configuring-lag-tolerance)
* [Configuring deferral](#configuring-deferral)

## Configuring lag tolerance[​](#configuring-lag-tolerance "Direct link to Configuring lag tolerance")

Lag tolerance allows you to set a tolerance level for older data at the project, environment, or model level. We recommend starting with the following Jinja expression, which tolerates older data locally and requires fresher data in production. As you get a better feel for where adjustments make sense, you can tune individual models.

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: "{{ '4h' if target.name == 'prod' else '7d' }}"
```

In this example, models in the `prod` target rebuild only when upstream data is more than 4 hours old. In all other environments, models wait 7 days before rebuilding.

For more details, refer to the [`lag_tolerance` config reference](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md).

## Configuring deferral[​](#configuring-deferral "Direct link to Configuring deferral")

By default, dbt State defers to your production environment. To customize which environment dbt defers to, use the [`defer_to_target`](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md) config.

For the full list of available configs, see [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md).

## Inviting team members[​](#inviting-team-members "Direct link to Inviting team members")

The more team members you have using dbt State, the better it gets; more team members means more opportunities to clone existing nodes rather than rebuilding them.

* **For [standalone app](https://app.state.dbt.com) users**: Click the invite link in the upper-right corner of the **Users** page.
* **For dbt platform users**: Have your colleagues run [`dbt login`](https://docs.getdbt.com/reference/commands/login.md) after dbt State is enabled on the account.

## Debugging dbt State[​](#debugging-dbt-state "Direct link to Debugging dbt State")

If dbt State is behaving unexpectedly, you can prepend your run command with the `DBT_ENGINE_MANAGE_STATE` environment variable to isolate the issue:

```bash
DBT_ENGINE_MANAGE_STATE=1 dbt run --target dev --select "customers"
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Non-interactive environment setup](https://docs.getdbt.com/docs/deploy/dbt-state-cicd.md)
* [Configuring deferral](https://docs.getdbt.com/docs/deploy/dbt-state-deferral.md)
* [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md)
* [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md)
* [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage)
