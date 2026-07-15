# About var function

You can provide variables from `vars.yml` or `dbt_project.yml` to models during compilation. These variables are useful for configuring packages for deployment in multiple environments, or for defining values that should be used across multiple models within a package.

Variables defined in `vars.yml` or `dbt_project.yml` act as project-wide defaults. You can override them at runtime using the `--vars` command-line argument. For example, when testing with a different date range or running models with environment-specific settings without changing your model logic.

To retrieve a variable inside a model, hook, or macro, use the `var()` function. The `var()` function returns the value defined in your project or passed using `--vars`, based on precedence.

You can use `var()` anywhere dbt renders Jinja during compilation, including most `.sql` and `.yml` files in your project. It does not work in configuration files that dbt reads before compilation, such as [`profiles.yml`](https://docs.getdbt.com/reference/dbt-jinja-functions/profiles-yml-context.md) or [`packages.yml`](<https://docs.getdbt.com/reference/dbt-jinja-functions/packages.yml context.md>).

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

<!-- -->

To define a variable in your project, add the `vars:` config to a dedicated `vars.yml` file or to your `dbt_project.yml` file. `vars.yml` is parsed *before* `dbt_project.yml`, so you can reference variables from `vars.yml` in `dbt_project.yml` using `{{ var('...') }}`.

* vars.yml
* dbt\_project.yml

vars.yml

```yaml
vars:
  event_type: activation
```

Beta feature

The `vars.yml` file is a beta feature in dbt Core v1.12.

You can define variables in `dbt_project.yml`, or reference variables from `vars.yml` (for example, in your `models` config):

dbt\_project.yml

```yaml
name: my_dbt_project
version: 1.0.0
config-version: 2

# Option 1: Define variables here
vars:
  event_type: activation

# Option 2: Reference a variable from vars.yml
models:
  my_dbt_project:
    +schema: "{{ var('event_type') }}"
```

You cannot define variables in both `vars.yml` and `dbt_project.yml`; you can only use one or the other. If both files contain a `vars` block with definitions, dbt raises an error.

See the docs on [using variables](https://docs.getdbt.com/docs/build/project-variables.md) for more information on how to define variables in your dbt project.

### Variable default values[​](#variable-default-values "Direct link to Variable default values")

The `var()` function takes an optional second argument, `default`. If this argument is provided, then it will be the default value for the variable if one is not explicitly defined.

my\_model.sql

```sql
-- Use 'activation' as the event_type if the variable is not defined.
select * from events where event_type = '{{ var("event_type", "activation") }}'
```

### Command line variables[​](#command-line-variables "Direct link to Command line variables")

<!-- -->

The `dbt_project.yml` file is a great place to define variables that rarely change.

When you need to override a variable for a specific run, use the `--vars` command line option. For example, when you want to test with a different date range, run models with environment-specific settings, or adjust behavior dynamically.

Use `--vars` to pass one or more variables to a dbt command. Provide the argument as a YAML dictionary string.

For example:

```shell
$ dbt run --vars '{"event_type": "signup"}'
```

You can use the same `--vars` syntax with other dbt commands, such as `dbt snapshot`:

```shell
$ dbt snapshot --select my_snapshot --vars '{"cutoff_date": "2026-01-01"}'
```

Inside a model, snapshot, or macro, access the value using the `var()` function:

```shell
select '{{ var("event_type") }}' as event_type
```

When you pass variables using `--vars`, you can access them anywhere you use the `var()` function in your project.

You can pass multiple variables at once:

```shell
$ dbt run --vars '{event_type: signup, region: us}'
```

If only one variable is being set, the brackets are optional:

```shell
$ dbt run --vars 'event_type: signup'
```

The `--vars` argument accepts a YAML dictionary as a string on the command line. YAML is convenient because it does not require strict quoting as with JSON.

Both of the following are valid and equivalent:

```shell
$ dbt run --vars '{"key": "value", "date": 20180101}'
$ dbt run --vars '{key: value, date: 20180101}'
```

Variables defined using `--var`, override values defined in `dbt_project.yml`. This makes `--vars` useful for temporarily overriding configuration without changing your committed project files. For the complete order of precedence (including package-scoped variables and default values defined in `var()`), see [Variable precedence](https://docs.getdbt.com/docs/build/project-variables.md#variable-precedence).
