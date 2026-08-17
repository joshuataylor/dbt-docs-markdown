# No + prefix on resource paths

Fusion-only flag

This behavior change flag is specific to Fusion and has no equivalent in dbt Core.

| require\_resource\_names\_without\_plus\_prefix | Fusion             |
| ----------------------------------------------- | ------------------ |
| Introduced                                      | 2.0.0-preview\.208 |
| Matured (default → `true`)                      | —                  |
| Removed                                         | —                  |

The `require_resource_names_without_plus_prefix` flag is set to `false` by default.

In `dbt_project.yml`, the [`+` prefix](../../resource-configs/plus-prefix.md) marks a config, while a key without the prefix is treated as part of a [resource path](../../resource-configs/resource-path.md) (a folder or file name). dbt historically accepted resource paths that start with `+` (for example, a folder named `+my_folder` under `models`) even though the `+` prefix is meant for configs only. This is accidental behavior that produces misleading error messages when a config is missing its `+` prefix, because dbt can't reliably tell whether a `+`-prefixed key is a folder name or a mistyped config.

When the `require_resource_names_without_plus_prefix` flag is set to `false`, dbt raises a warning for each resource path that starts with `+`:

```shell

[warning] [InvalidConfig (dbt1005)]: Resource path `snapshot_contract.+path_1` in dbt_project.yml starts with `+`. This will be deprecated in future versions of dbt.
```

When the flag is set to `true`, dbt treats a `+`-prefixed key as an unrecognized config key and raises an error instead:

```shell

[error] [SerializationError (dbt1013)]: Invalid model definition `snapshot_contract.+path_1`: Unrecognized key `snapshot_contract.+path_1`. Custom keys must go under `+meta`.
```

To resolve the warning or error, remove the + prefix from any folder or file name in dbt\_project.yml. For custom keys, nest them under [`+meta`](../../resource-configs/meta.md).

For example, name the resource path so it doesn't start with `+`:

dbt\_project.yml

```yml

models:
  my_project:
    my_folder:        # resource path — no + prefix
      +enabled: true  # config — keep the + prefix
```
