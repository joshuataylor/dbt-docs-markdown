# Print output

### Suppress `print()` messages in stdout[​](#suppress-print-messages-in-stdout "Direct link to suppress-print-messages-in-stdout")

By default, dbt includes [`print()`](https://docs.getdbt.com/reference/dbt-jinja-functions/print.md) messages in standard out (stdout). You can use the `DBT_PRINT` environment variable to prevent these messages from showing up in stdout.

Syntax deprecation

The original `DBT_NO_PRINT` environment variable has been deprecated, starting with dbt v1.5. Backward compatibility is supported but will be removed in an as-of-yet-undetermined future release.

Supply `--no-print` flag to `dbt run` to suppress `print()` messages from showing in stdout.

```text
dbt run --no-print
```

### Printer width[​](#printer-width "Direct link to Printer width")

By default, dbt will print out lines padded to 80 characters wide. You can change this setting by adding the following to your `profiles.yml` file:

profiles.yml

```yaml
config:
  printer_width: 120
```

### Print color[​](#print-color "Direct link to Print color")

By default, dbt will colorize the output it prints in your terminal. You can turn this off by adding the following to your `profiles.yml` file:

profiles.yml

```yaml
config:
  use_colors: False
```

```text
dbt run --use-colors
dbt run --no-use-colors
```

You can set the color preferences for the file logs only within `profiles.yml` or using the `--use-colors-file / --no-use-colors-file` flags.

profiles.yml

```yaml
config:
  use_colors_file: False
```

```text
dbt run --use-colors-file
dbt run --no-use-colors-file
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
