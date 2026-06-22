# About as\_native filter

The `as_native` Jinja filter will coerce Jinja-compiled output into its Python native representation according to [`ast.literal_eval`](https://docs.python.org/3/library/ast.html#ast.literal_eval). The result can be any Python native type (set, list, tuple, dict, etc).

To render boolean and numeric values, it is recommended to use [`as_bool`](https://docs.getdbt.com/reference/dbt-jinja-functions/as_bool.md) and [`as_number`](https://docs.getdbt.com/reference/dbt-jinja-functions/as_number.md) instead.

Proceed with caution

Unlike `as_bool` and `as_number`, `as_native` will return a rendered value regardless of the input type. Ensure that your inputs match expectations.
