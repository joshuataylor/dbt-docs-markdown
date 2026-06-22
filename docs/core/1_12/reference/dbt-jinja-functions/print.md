# About print function

Use the `print()` function when you want to print messages to both the log file and standard output (stdout).

When used in conjunction with the `QUIET` global config, which suppresses non-error logs, you will only see error logs and the print messages in stdout. For more information, see [Global configs](https://docs.getdbt.com/reference/global-configs/about-global-configs.md).

## Example[​](#example "Direct link to Example")

```sql
  {% macro some_macro(arg1, arg2) %}
    {{ print("Running some_macro: " ~ arg1 ~ ", " ~ arg2) }}
  {% endmacro %}
```
