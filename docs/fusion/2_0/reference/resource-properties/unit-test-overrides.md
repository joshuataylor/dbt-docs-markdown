# Unit test overrides

When configuring your unit test, you can override the output of [macros](https://docs.getdbt.com/docs/build/jinja-macros.md#macros), [project variables](https://docs.getdbt.com/docs/build/project-variables.md), or [environment variables](https://docs.getdbt.com/docs/build/environment-variables.md) for a given unit test.

models/schema.yml

```yml

 - name: test_my_model_overrides
    model: my_model
    given:
      - input: ref('my_model_a')
        rows:
          - {id: 1, a: 1}
      - input: ref('my_model_b')
        rows:
          - {id: 1, b: 2}
          - {id: 2, b: 2}
    overrides:
      macros:
        type_numeric: override
        invocation_id: 123
      vars:
        my_test: var_override
      env_vars:
        MY_TEST: env_var_override
    expect:
      rows:
        - {macro_call: override, var_call: var_override, env_var_call: env_var_override, invocation_id: 123}
```

## Macros[​](#macros "Direct link to Macros")

You can override the output of any macro referenced directly by the model being unit tested in your unit test definition. Overrides apply only to macros, variables, and environment variables that are referenced directly within the model being unit tested. If a macro, variable, or environment variable is only referenced indirectly (for example, inside a macro that your model calls), the override will not be applied.

If the model you're unit testing uses these macros, you must override them:

* [`is_incremental`](https://docs.getdbt.com/docs/build/incremental-models.md#understand-the-is_incremental-macro): If you're unit testing an incremental model, you must explicitly set `is_incremental` to `true` or `false`. See more docs on unit testing incremental models [here](https://docs.getdbt.com/docs/build/unit-tests.md#unit-testing-incremental-models).

models/schema.yml

```yml

unit_tests:
  - name: my_unit_test
    model: my_incremental_model
    overrides:
      macros:
        # unit test this model in "full refresh" mode
        is_incremental: false 
    ...
```

* [`dbt_utils.star`](https://docs.getdbt.com/blog/star-sql-love-letter): If you're unit testing a model that uses the `star` macro, you must explicitly set `star` to a list of columns. This is because the `star` only accepts a [relation](https://docs.getdbt.com/reference/dbt-classes.md#relation) for the `from` argument; the unit test mock input data is injected directly into the model SQL, replacing the `ref('')` or `source('')` function, causing the `star` macro to fail unless overridden.

models/schema.yml

```yml

unit_tests:
  - name: my_other_unit_test
    model: my_model_that_uses_star
    overrides:
      macros:
        # explicitly set star to relevant list of columns
        dbt_utils.star: col_a,col_b,col_c 
    ...
```
