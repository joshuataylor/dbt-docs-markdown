# About dbt snapshot command

The `dbt snapshot` command executes the [Snapshots](https://docs.getdbt.com/docs/build/snapshots.md) defined in your project. Snapshots record changes to your source data over time by implementing [type-2 Slowly Changing Dimensions](https://en.wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row). Run `dbt snapshot` on a schedule (for example, daily) to capture changes in your source tables.

Define snapshots in YAML with a strategy and `unique_key`; refer to [Snapshot configurations](https://docs.getdbt.com/reference/snapshot-configs.md) for details on how to set them up. You can also run snapshots as part of [dbt build](https://docs.getdbt.com/reference/commands/build.md).

dbt looks for snapshots in the directories listed in `snapshot-paths` in your `dbt_project.yml` file. By default, dbt uses the `snapshots/` directory. You can specify multiple paths if you organize snapshots in more than one folder.

## Usage[​](#usage "Direct link to Usage")

To view the full list of supported options in your terminal, run:

```shell
dbt snapshot --help
```

Use `--select` or `--exclude` to choose which snapshots run. For selection syntax, refer to [Node selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md). `dbt snapshot` also supports common command-line options, such as `--target` and `--threads`. For flag details (including logging options), refer to [About flags (global configs)](https://docs.getdbt.com/reference/global-configs/about-global-configs.md).

Use `--vars` when your snapshot SQL references values with the `var()` function. For syntax, precedence, and more examples, refer to [Defining variables on the command line](https://docs.getdbt.com/docs/build/project-variables.md#defining-variables-on-the-command-line).

```shell
dbt snapshot --select my_snapshot --vars '{"cutoff_date": "2026-01-01"}'
```

<!-- -->

Compiled SQL for snapshots[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Starting dbt Core v1.12, you can inspect the SQL generated for this snapshot by running [`dbt compile`](https://docs.getdbt.com/reference/commands/compile.md) or `dbt compile --select orders_snapshot`.

Open the compiled SQL in `target/compiled/` to inspect or debug the generated queries. Each snapshot is compiled into its own SQL file, even if multiple snapshots are defined in the same source file.
