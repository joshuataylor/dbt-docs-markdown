# How is dbt State different from using state:modified?

`state:modified` in dbt Core requires manual management of `manifest.json`, which is cumbersome and error-prone. dbt State is completely managed with almost zero setup and no workflow changes.

`state:modified` only checks if a file has changed. dbt State has semantic understanding of SQL, so meaningless changes like whitespace or table aliases are not counted as a change — making dbt State smarter about what actually needs to rebuild.

`state:modified` does not consider upstream data changes. dbt State checks all sources to see if there is any new data or if the schema has been modified. This enables dbt State to skip running models if the result of the run would be the same as before. For example, if you run `dbt run` with `state:modified` twice, it runs all modified models both times. dbt State only reruns models the second time if upstream sources have changed.

dbt State also has the ability to auto-defer refs and automatically clone tables when the result of the clone would have been the same as a full model run.

`state:modified` has a limitation on seed files over 1MB, while dbt State does not.
