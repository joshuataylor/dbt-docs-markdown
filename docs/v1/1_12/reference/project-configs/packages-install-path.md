# packages-install-path

dbt\_project.yml

```yml
packages-install-path: directorypath
```

## Definition

Optionally specify a custom directory where [packages](../../docs/build/packages.md) are installed when you run the `dbt deps` [command](../commands/deps.md). Note that this directory is usually git-ignored.

## Default

By default, dbt will install packages in the `dbt_packages` directory, i.e. `packages-install-path: dbt_packages`

## Examples

### Install packages in a subdirectory named `packages` instead of `dbt_packages`

dbt\_project.yml

```yml
packages-install-path: packages
```
