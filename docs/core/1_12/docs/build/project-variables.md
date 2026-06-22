# Project variables

dbt provides a mechanism called [variables](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md) to provide data to models for compilation. Variables allow you to define configurable values for your project instead of hardcoding them in SQL.

<!-- -->

You can define variables in three ways:

* In the `vars.yml` file (recommended for projects with several variables, or when you need to reference variables from within [`dbt_project.yml`](https://docs.getdbt.com/reference/dbt_project.yml.md))
* In the `dbt_project.yml` file
* On the command line

Variables defined in the `vars.yml` or `dbt_project.yml` act as project-wide defaults. These defaults apply anywhere the variable is referenced. You can override them at runtime using the `--vars` command-line argument, which is useful when you want to change a value for a specific run. For example, when testing with a different date range or running models with environment-specific settings.

You might use variables to [configure timezones](https://github.com/dbt-labs/snowplow/blob/0.3.9/dbt_project.yml#L22), set reporting date ranges, [avoid hardcoding table names](https://github.com/dbt-labs/quickbooks/blob/v0.1.0/dbt_project.yml#L23), or otherwise control how models are compiled.

To use a variable in a model, hook, or macro, use the `{{ var('...') }}` function. The `var()` function retrieves the value defined in your project or passed using `--vars`. For more information, see [About var function](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md).

Note, refer to [YAML tips](https://docs.getdbt.com/docs/build/dbt-tips.md#yaml-tips) for more YAML information.

### Defining variables in `vars.yml`[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#defining-variables-in-varsyml "Direct link to defining-variables-in-varsyml")

Beta feature

The `vars.yml` file is a beta feature in dbt Core v1.12.

`vars.yml` is a dedicated file for project variables located at the project root. Because dbt parses it *before* `dbt_project.yml`, variables from `vars.yml` are available when `dbt_project.yml` is evaluated, so you can reference them in `dbt_project.yml` using `{{ var('...') }}`. For more information, refer to [Using vars.yml variables in dbt\_project.yml](#using-varsyml-variables-in-dbt_projectyml).

To define variables in a `vars.yml` file, add a `vars` config. You can scope these variables globally, or to a specific package imported in your project. The following example shows variables in a `vars.yml` file:

vars.yml

```yaml
vars:
  # The `start_date` variable is accessible to all resources
  start_date: '2016-06-01'
  
  # The `platforms` variable is only accessible to resources in the my_dbt_project project
  my_dbt_project:
    platforms: ['web', 'mobile']
  
  # The `app_ids` variable is only accessible to resources in the snowplow package
  snowplow:
    app_ids: ['marketing', 'app', 'landing-page']
```

You cannot define variables in both `vars.yml` and `dbt_project.yml`. If both files contain a `vars` block with definitions, dbt raises an error.

If `vars.yml` is empty or doesn't exist, variables defined in `dbt_project.yml` are used instead.

#### Using `vars.yml` variables in `dbt_project.yml`[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#using-varsyml-variables-in-dbt_projectyml "Direct link to using-varsyml-variables-in-dbt_projectyml")

Because `vars.yml` is parsed before `dbt_project.yml`, you can reference variables from `vars.yml` within `dbt_project.yml` using `{{ var('...') }}`. Variables referenced in `dbt_project.yml` are resolved only from `vars.yml` or the `--vars` command-line argument, not from the `vars` block in `dbt_project.yml` itself. If `dbt_project.yml` references a variable that is defined only in its own `vars` block, dbt raises an error.

For example, you have the following variables defined in `vars.yml`:

vars.yml

```yaml
vars:
  schema_name: analytics
  materialization: table
```

You can reference them in `dbt_project.yml`:

dbt\_project.yml

```yaml
models:
  my_dbt_project:
    +schema: "{{ var('schema_name') }}"
    +materialized: "{{ var('materialization') }}"
```

### Defining variables in `dbt_project.yml`[​](#defining-variables-in-dbt_projectyml "Direct link to defining-variables-in-dbt_projectyml")

info

Jinja is not supported within the `vars` config, and all values will be interpreted literally.

To define variables in a dbt project, add a `vars` config to your `dbt_project.yml` file. These `vars` can be scoped globally, or to a specific package imported in your project.

dbt\_project.yml

```yaml
name: my_dbt_project
version: 1.0.0

config-version: 2

vars:
  # The `start_date` variable will be accessible in all resources
  start_date: '2016-06-01'

  # The `platforms` variable is only accessible to resources in the my_dbt_project project
  my_dbt_project:
    platforms: ['web', 'mobile']

  # The `app_ids` variable is only accessible to resources in the snowplow package
  snowplow:
    app_ids: ['marketing', 'app', 'landing-page']

models:
    ...
```

You cannot define variables in both `vars.yml` and `dbt_project.yml`. If both files contain a `vars` block with definitions, dbt raises an error.

### Defining variables on the command line[​](#defining-variables-on-the-command-line "Direct link to Defining variables on the command line")

<!-- -->

The `dbt_project.yml` file is a great place to define variables that rarely change.

When you need to override a variable for a specific run, use the `--vars` command line option. For example, when you want to test with a different date range, run models with environment-specific settings, or adjust behavior dynamically.

Use `--vars` to pass one or more variables to a dbt command. Provide the argument as a YAML dictionary string.

For example:

```text
$ dbt run --vars '{"event_type": "signup"}'
```

Inside a model or macro, access the value using the `var()` function:

```text
select '{{ var("event_type") }}' as event_type
```

When you pass variables using `--vars`, you can access them anywhere you use the `var()` function in your project.

You can pass multiple variables at once:

```text
$ dbt run --vars '{event_type: signup, region: us}'
```

If only one variable is being set, the brackets are optional:

```text
$ dbt run --vars 'event_type: signup'
```

The `--vars` argument accepts a YAML dictionary as a string on the command line. YAML is convenient because it does not require strict quoting as with JSON.

Both of the following are valid and equivalent:

```text
$ dbt run --vars '{"key": "value", "date": 20180101}'
$ dbt run --vars '{key: value, date: 20180101}'
```

Variables defined using `--var`, override values defined in `dbt_project.yml`. This makes `--vars` useful for temporarily overriding configuration without changing your committed project files. For the complete order of precedence (including package-scoped variables and default values defined in `var()`), see [Variable precedence](https://docs.getdbt.com/docs/build/project-variables.md#variable-precedence).

You can find more information on defining dictionaries with YAML [here](https://github.com/Animosity/CraftIRC/wiki/Complete-idiot%27s-introduction-to-yaml).

### Variable precedence[​](#variable-precedence "Direct link to Variable precedence")

<!-- -->

Variables defined with the `--vars` command line argument override variables defined in `vars.yml` or `dbt_project.yml`. They are globally scoped and accessible to the root project and all installed packages.

The order of precedence for variable declaration is as follows (highest priority first):

1. The variables defined on the command line with `--vars`
2. The package-scoped variable declaration in the root `vars.yml` file (if present)
3. The global variable declaration in the root `vars.yml` file (if present)
4. The package-scoped variable declaration in the root `dbt_project.yml` file
5. The global variable declaration in the root `dbt_project.yml` file
6. If this node is defined in a package: variable declarations in that package's `dbt_project.yml` file
7. The variable's default argument (if one is provided)

`vars.yml` is parsed *before* `dbt_project.yml`, so variables defined in `vars.yml` are available when `dbt_project.yml` is parsed. If `vars.yml` is empty or doesn't exist, variables from `dbt_project.yml` are used instead. When any variable (in `vars.yml`, `dbt_project.yml`, or from the command line) changes, partial parsing is disabled for the next run.

If dbt is unable to find a definition for a variable after checking all possible variable declaration places, then a compilation error will be raised.

**Note:** Variable scope is based on the node ultimately using that variable. Imagine the case where a model defined in the root project is calling a macro defined in an installed package. That macro, in turn, uses the value of a variable. The variable will be resolved based on the *root project's* scope, rather than the package's scope.
