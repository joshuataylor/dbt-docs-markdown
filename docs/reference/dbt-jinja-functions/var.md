# About var function

Variables can be passed from your `dbt_project.yml` file into models during compilation. These variables are useful for configuring packages for deployment in multiple environments, or defining values that should be used across multiple models within a package.

To add a variable to a model, use the `var()` function:

my\_model.sql

```sql
select * from events where event_type = '{{ var("event_type") }}'
```

If you try to run this model without supplying an `event_type` variable, you'll receive a compilation error that looks like this:

```text
Encountered an error:
! Compilation error while compiling model package_name.my_model:
! Required var 'event_type' not found in config:
Vars supplied to package_name.my_model = {
}
```

To define a variable in your project, add the `vars:` config to your `dbt_project.yml` file. See the docs on [using variables](https://docs.getdbt.com/docs/build/project-variables.md) for more information on defining variables in your dbt project.

dbt\_project.yml

```yaml
name: my_dbt_project
version: 1.0.0

config-version: 2

# Define variables here
vars:
  event_type: activation
```

### Variable default values[​](#variable-default-values "Direct link to Variable default values")

The `var()` function takes an optional second argument, `default`. If this argument is provided, then it will be the default value for the variable if one is not explicitly defined.

my\_model.sql

```sql
-- Use 'activation' as the event_type if the variable is not defined.
select * from events where event_type = '{{ var("event_type", "activation") }}'
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
