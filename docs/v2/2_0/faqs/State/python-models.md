# Does dbt State support Python models?

dbt State builds Python models but does not reuse them. It executes Python models on every run, even when their code and upstream data have not changed.
