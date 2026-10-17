# Enabling dbt State on environments and jobs

Login required | Usage-based

Learn how to enable [dbt State](./dbt-state-about.md) on deployment environments and individual jobs. This option is only visible if dbt State is [enabled on your account](./dbt-state-setup.md).

## Enabling dbt State on environments

You can enable [dbt State](./dbt-state-about.md) at the environment level, allowing jobs in that environment to inherit the setting automatically.

When you enable dbt State on a deployment environment (production, staging, or general):

* New jobs default to **Inherited from environment** and have dbt State enabled without any additional configuration.
* Existing jobs are not automatically updated. You must [configure each job manually](#enabling-dbt-state-on-individual-jobs).

To enable dbt State on a deployment environment:

1. Go to **Orchestration** > **Environments**.
2. Select the environment you want to enable dbt State for.
3. Click **Settings** > **Edit**.
4. In the **dbt State** section, select **Enable dbt State**.
5. Click **Save**.

For development environments, refer to [Enabling dbt State in Studio](./dbt-state-enable-studio.md).

## Enabling dbt State on individual jobs

dbt State is available on all job types: deploy, continuous integration (CI), and merge jobs. Each job has a **dbt State** dropdown in its execution settings with **On**, **Off**, or **Inherited from environment** options. New jobs default to **Inherited from environment** — no additional configuration needed as long as dbt State is enabled on the environment.

Existing jobs default to **Off**. To follow the environment's setting, you must configure each job manually.

To enable dbt State on individual jobs:

1. Go to **Orchestration** > **Jobs**.
2. Select the job you want to enable dbt State for.
3. Click **Settings** > **Edit**.
4. In the **Execution settings** section, set the **dbt State** dropdown to **On** to enable it explicitly, or **Inherited from environment** to follow the environment's setting. If you select **Inherited from environment**, make sure dbt State is [enabled on the environment](#enabling-dbt-state-on-environments) first — otherwise, the job will inherit it as off.
5. Click **Save**.

note

dbt State and State-aware orchestration are mutually exclusive. Selecting **Inherited from environment** or **On** automatically clears the **State-aware orchestration** option, and enabling **State-aware orchestration** sets **dbt State** to **Off**.

## Related docs

* [About dbt State](./dbt-state-about.md)
* [Set up dbt State](./dbt-state-setup.md)
* [Enable dbt State in Studio](./dbt-state-enable-studio.md)
