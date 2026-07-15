# Source definitions for state:modified

Removed in dbt Core v2

This flag was removed in dbt Core v2 and in Fusion. The new behavior is always enabled. If you're upgrading, remove this flag from your `dbt_project.yml`.

<!-- -->

| state\_modified\_compare\_more\_unrendered\_values | dbt **Latest** | dbt Core |
| -------------------------------------------------- | -------------- | -------- |
| Introduced                                         | 2024.10        | 1.9.0    |
| Matured (default → `true`)                         | Sep 1, 2026    | —        |
| Removed                                            | —              | v2.0     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

info

You need to build the state directory using dbt v1.9 or higher, or [the dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md), and you need to set `state_modified_compare_more_unrendered_values` to `true` within your dbt\_project.yml.

If the state directory was built with an older dbt version or if the `state_modified_compare_more_unrendered_values` behavior change flag was either not set or set to `false`, you need to rebuild the state directory to avoid false positives during state comparison with `state:modified`.

Set `state_modified_compare_more_unrendered_values` to `true` to reduce false positives during `state:modified` checks, especially when configs differ by target environment (such as `prod` vs. `dev`).

Setting the flag to `true` changes the `state:modified` comparison from using rendered values to unrendered values instead. It accomplishes this by persisting `unrendered_config` during model parsing and `unrendered_database` and `unrendered_schema` configs during source parsing.

note

This flag requires rebuilding the state directory (manifest) to take effect.

## Impact when the flag matures[​](#impact-when-the-flag-matures "Direct link to Impact when the flag matures")

Setting the default to `true` silently changes the `state:modified` selection set that most CI, Slim CI, and `dbt build --defer` workflows rely on. There are two ways this surfaces:

* **False "modified" on the first run after the flag is set to `true`.** If the baseline manifest was captured before the flag is set (rendered values stored) and the current parse runs after the setting change (literal text stored), every node whose YAML config contains Jinja will appear as `state:modified`, even if nothing has changed. This causes a full rebuild on the first CI run after the upgrade.
* **New positives going forward.** After both manifests are captured with the flag enabled, `state:modified` will catch cases where two equivalent Jinja expressions render to the same value (for example, switching from `"{{ env_var('MAT', 'view') }}"` to `view`).

 What to expect

On the first CI or Slim CI run after the flag is set, any node whose YAML config uses Jinja (`env_var`, `var`, conditional materialization) may appear as `state:modified` even if nothing changed. This is because the baseline manifest stored rendered values while the new parse stores literal Jinja text — the two sides of the comparison differ on serialization, not on real changes.

Once your production job runs once on the **Latest** release track and generates a new baseline manifest, both sides of the `state:modified` comparison use the same format and the extra diffs disappear.

No code change is required. The cost is one extra rebuild cycle for affected nodes.

This new behavior fixes the legacy behavior. Previously, the comparison silently missed real changes to Jinja expressions (for example, switching from `"{{ env_var('MAT', 'view') }}"` to `view` renders identically and was never caught). With this flag enabled, `state:modified` is more accurate going forward.

To opt out, set the flag to `false` in `dbt_project.yml`:

dbt\_project.yml

```yaml
flags:
  state_modified_compare_more_unrendered_values: false
```

Opting out is recommended during a production deploy freeze or if your project has heavy Jinja in YAML configs and the one-time rebuild would exceed your CI compute budget. Otherwise, enabling this flag is recommended.
