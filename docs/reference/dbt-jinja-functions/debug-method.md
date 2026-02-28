# About debug macro

Requires Core CLI

The `debug()` macro is only available when using dbt Core CLI in a local development environment. It's *not available* in dbt platform.

Do not deploy code to production that uses the `debug` macro.

If developing in dbt platform or using Fusion, you can instead use:

* [`{{ print() }}`](https://docs.getdbt.com/reference/dbt-jinja-functions/print.md) - Print messages to both the log file and standard output (`stdout`).
* [`{{ log() }}`](https://docs.getdbt.com/reference/dbt-jinja-functions/log.md) - Structured logging that prints messages during Jinja rendering.

The `{{ debug() }}` macro will open an iPython debugger in the context of a compiled dbt macro. The `DBT_MACRO_DEBUGGING` environment variable must be set to use the debugger.

This function requires:

* Interactive terminal access with iPython debugger (`ipdb`) installed. Fusion doesn't provide a iPython (ipdb) debugger since its built on Rust. It instead outputs a non-interactive snapshot of the MiniJinja render context in the compiled code.
* Local development environment running dbt Core CLI
* `DBT_MACRO_DEBUGGING` environment variable set

## Usage[​](#usage "Direct link to Usage")

my\_macro.sql

```text

{% macro my_macro() %}

  {% set something_complex = my_complicated_macro() %}
  
  {{ debug() }}

{% endmacro %}
```

When dbt hits the `debug()` line, you'll see something like:

```shell
$ DBT_MACRO_DEBUGGING=write dbt compile
Running with dbt=1.0
> /var/folders/31/mrzqbbtd3rn4hmgbhrtkfyxm0000gn/T/dbt-macro-compiled-cxvhhgu7.py(14)root()
     13         environment.call(context, (undefined(name='debug') if l_0_debug is missing else l_0_debug)),
---> 14         environment.call(context, (undefined(name='source') if l_0_source is missing else l_0_source), 'src', 'seedtable'),
     15     )

ipdb> l 9,12
      9     l_0_debug = resolve('debug')
     10     l_0_source = resolve('source')
     11     pass
     12     yield '%s\nselect * from %s' % (
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
