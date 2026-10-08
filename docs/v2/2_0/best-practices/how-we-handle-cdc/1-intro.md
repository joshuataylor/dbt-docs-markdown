# Change data capture in dbt

Change data capture (CDC) identifies new, updated, and deleted rows in your source data so you can process changes without rebuilding an *entire* table.

This guide explains how you can use incremental models and snapshots in dbt to keep tables current, preserve a history of changes, or both. To find the best approach for your project, refer to [Choosing incremental models or snapshots](./2-choosing-incremental-or-snapshots.md). For job frequency, streams, and dynamic tables, refer to [Near real-time data in dbt](../how-we-handle-real-time-data/1-intro.md), including [CDC with Snowflake Streams](../how-we-handle-real-time-data/2-incremental-patterns.md#cdc-with-snowflake-streams).

## How to handle CDC in dbt

When source data changes, you may want to update your table, keep old versions, or do both.

In dbt, that usually means one of two ways to build a table:

* An [incremental model](../../docs/build/incremental-models-overview.md) keeps a table current. On each run, dbt processes new or changed rows and *replaces* the old row for that `unique_key`.
* A [snapshot](../../docs/build/snapshots.md) keeps history. On each run, dbt compares the source to the last snapshot and *adds* a row when the record changes, with `dbt_valid_from` and `dbt_valid_to`.

Snapshots only capture changes when you run them, which means you should run them on a schedule or you might miss changes. Refer to the FAQ [How often should I run the snapshot command?](../../faqs/Runs/snapshot-frequency.md), which recommends hourly to daily.

## Choose an approach: latest row, history, or both

Your approach depends on how your source exposes changes and what you need to keep.

| If you need                                                              | How your source stores changes                                                        | Use                                                                                                            |
| ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Latest row only                                                          | Appends a row for each change, or overwrites rows but has a reliable change timestamp | Incremental model                                                                                              |
| Current row plus old versions                                            | Overwrites rows, and is small enough to scan each run                                 | Snapshot                                                                                                       |
| Current row plus old versions, without scanning the full source each run | Overwrites rows, or appends a row for each change (after cleanup)                     | An incremental staging model, then a snapshot, then a downstream model that keeps only the latest snapshot row |

For examples of each approach, refer to [Choosing incremental models or snapshots](./2-choosing-incremental-or-snapshots.md).

## Key recommendations

* Use incremental models when you only need the current rows and you can identify new or changed rows. An incremental model *replaces* the old row, so runs stay small and you do not store versions you will never query.
* Use snapshots when you need to know what a record looked like at a point in the past. A snapshot *adds* a row when the record changes, which is how you keep the old version. An incremental model would have overwritten it.
* Use both when staging should stay cheap and current, and a snapshot should store versions. The incremental model limits how much you process. The snapshot records history. Snapshot the staging models (or sources), not the final table people query, so you track the source as it changed, not a report that can change for other reasons.
* Prefer a snapshot `timestamp` strategy when `updated_at` is reliable and only moves forward. That lets dbt detect a change from the clock instead of comparing every column. Use `check` when the timestamp is missing or untrustworthy, so a change in the row still gets recorded.
* If the warehouse already writes a list of changes (streams), use an incremental model. The warehouse already detected the change, so you do not need a snapshot to find it. Refer to [CDC with Snowflake Streams](../how-we-handle-real-time-data/2-incremental-patterns.md#cdc-with-snowflake-streams) for that example.
