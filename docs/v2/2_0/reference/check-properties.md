# Check properties

Available in v2

You can declare check properties in `.yml` files in your `checks/` directory (as defined by the [`check-paths` config](./project-configs/check-paths.md)).

checks/\_checks.yml

```yaml
checks:
  - name: <string>
    description: <markdown_string>
    config:
      <check_config>: <config_value>

  - name: ... # declare properties of additional checks
```

## Example

checks/\_checks.yml

```yaml
version: 2

checks:
  - name: all_models_have_descriptions
    description: "Fails if any model is missing a description."
    config:
      severity: error
      tags: ["governance"]
      meta:
        owner: "data-platform-team"

  - name: public_models_have_owners
    description: "Warns if any public model is missing an owner."
    config:
      severity: warn
```
