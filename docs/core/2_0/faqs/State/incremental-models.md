# Does dbt State support incremental models?

dbt State works with incremental models. When you make a change to an incremental model and run it in development, dbt State automatically clones the model from production if it exists, then runs the new model logic on top of the clone.

If you want to revert to the original dbt behavior and fully refresh the incremental model, pass the [`--full-refresh` flag](https://docs.getdbt.com/reference/commands/run.md#refresh-incremental-models).
