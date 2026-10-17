# check-paths

Available in v2

dbt\_project.yml

```yml
check-paths: [directorypath]
```

Report incorrect code

## Definition

Specify custom directories where dbt looks for [checks](../../docs/build/checks.md).

## Default

By default, dbt looks for checks in the `checks` directory.

dbt\_project.yml

```yml
check-paths: ["checks"]
```

Report incorrect code

## Examples

Use a subdirectory named `project_rules` instead of `checks`:

dbt\_project.yml

```yml
check-paths: ["project_rules"]
```

Report incorrect code

Use multiple directories:

dbt\_project.yml

```yml
check-paths: ["checks", "shared_checks"]
```

Report incorrect code
