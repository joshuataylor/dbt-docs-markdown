# Does dbt State support incremental models?

dbt State works with incremental models. When you make a change to an incremental model and run it in development, dbt State automatically clones the existing model from production if it exists, then runs the new model logic on top of the clone. Use the [`pre_clone` config](../../reference/resource-configs/pre-clone.md) to change if and how incremental models and snapshots are populated before a run.

If you use the [`--full-refresh` flag](../../reference/commands/run.md#refresh-incremental-models), dbt State ignores any `always` and `if_missing` `pre_clone` configurations. However, if another schema already contains an equivalent version of the requested table, dbt State still clones that instead of building from scratch.
