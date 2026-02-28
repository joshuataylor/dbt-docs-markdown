# About modules variable

The `modules` variable in the Jinja context contains useful Python modules for operating on data.

## datetime[​](#datetime "Direct link to datetime")

This variable is a pointer to the Python [datetime](https://docs.python.org/3/library/datetime.html) module, which supports complex date and time logic.

It includes the modules contexts of `date`, `datetime`, `time`, `timedelta`, and `tzinfo`.

**Usage**

```text
{% set now = modules.datetime.datetime.now() %}
{% set three_days_ago_iso = (now - modules.datetime.timedelta(3)).isoformat() %}
```

This module will return the current date and time on every Jinja evaluation. For the date and time of the start of the run, please see [run\_started\_at](https://docs.getdbt.com/reference/dbt-jinja-functions/run_started_at.md).

## pytz[​](#pytz "Direct link to pytz")

This variable is a pointer to the Python [pytz](https://pypi.org/project/pytz/) module, which supports timezone logic.

**Usage**

```text
{% set dt = modules.datetime.datetime(2002, 10, 27, 6, 0, 0) %}
{% set dt_local = modules.pytz.timezone('US/Eastern').localize(dt) %}
{{ dt_local }}
```

## re[​](#re "Direct link to re")

This variable is a pointer to the Python [re](https://docs.python.org/3/library/re.html) module, which supports regular expressions.

**Usage**

```text
{% set my_string = 's3://example/path' %}
{% set s3_path_pattern = 's3://[a-z0-9-_/]+' %}

{% set re = modules.re %}
{% set is_match = re.match(s3_path_pattern, my_string, re.IGNORECASE) %}
{% if not is_match %}
    {%- do exceptions.raise_compiler_error(
        my_string ~ ' is not a valid s3 path'
    ) -%}
{% endif %}
```

## itertools[​](#itertools "Direct link to itertools")

Note

Starting in `dbt-core==1.10.6`, using `modules.itertools` raises a deprecation warning. For more information and suggested workarounds, refer to the [documentation on `ModulesItertoolsUsageDeprecation`](https://docs.getdbt.com/reference/deprecations.md#modulesitertoolsusagedeprecation).

This variable is a pointer to the Python [itertools](https://docs.python.org/3/library/itertools.html) module, which includes useful functions for working with iterators (loops, lists, and the like).

The supported functions are:

* `count`
* `cycle`
* `repeat`
* `accumulate`
* `chain`
* `compress`
* `islice`
* `starmap`
* `tee`
* `zip_longest`
* `product`
* `permutations`
* `combinations`
* `combinations_with_replacement`

**Usage**

```text
{%- set A = [1, 2] -%}
{%- set B = ['x', 'y', 'z'] -%}
{%- set AB_cartesian = modules.itertools.product(A, B) -%}

{%- for item in AB_cartesian %}
  {{ item }}
{%- endfor -%}
```

```text
  (1, 'x')
  (1, 'y')
  (1, 'z')
  (2, 'x')
  (2, 'y')
  (2, 'z')
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
