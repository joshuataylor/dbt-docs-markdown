# 20 docs tagged with "dbt State"

[View all tags](../tags.md)

## [About dbt State](../docs/deploy/dbt-state-about.md)

Learn about dbt State, its benefits, and key concepts for running only what has changed in your dbt project.

## [allow\_clones](../reference/resource-configs/allow-clones.md)

Configure whether dbt State makes clone decisions when running against a target.

## [compare\_unrendered\_code](../reference/resource-configs/compare-unrendered-code.md)

Controls whether dbt State checks both the Jinja template (unrendered code) and rendered SQL when deciding whether a model has changed.

## [Configuring deferral in dbt State](../docs/deploy/dbt-state-deferral.md)

Configure which environment dbt State defers to, including project and org disambiguation and deferral target customization.

## [dbt State configurations](../reference/resource-configs/dbt-state-configs.md)

Reference for all dbt State configurations — node-level state configs and profile-level settings.

## [dbt State trial and billing](../docs/deploy/dbt-state-trial.md)

Learn about dbt State trial and billing.

## [dbt State usage examples](../docs/deploy/dbt-state-examples.md)

Side-by-side dbt Core and dbt Core with dbt State execution scenarios using the Jaffle Shop project.

## [defer\_to\_target](../reference/resource-configs/defer-to-target.md)

Configure which target environment dbt State defers to for self-managed deployments.

## [Enabling dbt State in Studio](../docs/deploy/dbt-state-enable-studio.md)

Enable dbt State in the dbt Studio IDE for faster development runs, either at the environment level or per user.

## [Enabling dbt State on individual jobs](../docs/deploy/dbt-state-enable-jobs.md)

Enable dbt State on specific jobs in dbt platform, whether existing or newly created.

## [evaluate\_volatile\_sql](../reference/resource-configs/evaluate-volatile-sql.md)

Configure whether dbt State stores and compares the runtime results of volatile SQL functions when deciding whether to rebuild a node.

## [execute\_hooks\_on\_any\_reuse](../reference/resource-configs/execute-hooks-on-any-reuse.md)

Configure whether pre- and post-hooks run when dbt State skips a node because it's still fresh.

## [lag\_tolerance](../reference/resource-configs/lag-tolerance.md)

Configure lag\_tolerance to prevent unnecessary node rebuilds when upstream data updates more frequently than your node needs to.

## [metadata\_warehouse](../reference/resource-configs/metadata-warehouse.md)

Specify a dedicated Snowflake warehouse for dbt State metadata lookups to avoid queueing on your main compute warehouse.

## [Migrating from state-aware orchestration to dbt State](../docs/deploy/dbt-state-migration.md)

Step-by-step guide for migrating from state-aware orchestration to dbt State.

## [Monitoring dbt State activity in dbt platform](../docs/deploy/dbt-state-interface.md)

Learn how to monitor dbt State activity in dbt platform for better visibility into model builds and cost savings.

## [Non-interactive environment setup for dbt State](../docs/deploy/dbt-state-cicd.md)

Learn how to configure dbt State authentication for CI/CD and other non-interactive environments using service tokens or OAuth client credentials.

## [pre\_clone](../reference/resource-configs/pre-clone.md)

Configure when dbt State pre-populates incremental models and snapshots in dev by cloning their production counterparts.

## [require\_fresh\_data\_from](../reference/resource-configs/require-fresh-data-from.md)

Configure how many direct parent nodes need fresh data before a node is rebuilt by dbt State.

## [Setting up dbt State](../docs/deploy/dbt-state-setup.md)

Learn how to install and configure dbt State across dbt Core, dbt platform, and Fusion.
