# function-paths

dbt\_project.yml

```yml
function-paths: [directorypath]
```

Report incorrect code

## Definition

Optionally specify a custom list of directories where [user-defined functions (UDFs)](../../docs/build/udfs.md) are located.

## Default

By default, dbt will search for functions in the `functions` directory, for example, `function-paths: ["functions"]`

## Examples

Use a subdirectory named `udfs` instead of `functions`:

dbt\_project.yml

```yml
function-paths: ["udfs"]
```

Report incorrect code

Use multiple directories to organize your functions:

dbt\_project.yml

```yml
function-paths: ["functions", "custom_udfs"]
```

Report incorrect code
