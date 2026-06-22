# SQL DATEADD

If you’ve ever used the DATEADD SQL function across dialects (such as BigQuery, Postgres and Snowflake), you’ve probably had to google the syntax of the function every time. It's almost impossible to remember the argument order (or exact function name) of dateadd.

This article will go over how the DATEADD function works, the nuances of using it across the major cloud warehouses, and how to standardize the syntax variances using dbt macro.

## What is the DATEADD SQL function?[​](#what-is-the-dateadd-sql-function "Direct link to What is the DATEADD SQL function?")

The DATEADD function in SQL adds a time/date interval to a date and then returns the date. This allows you to add or subtract a certain period of time from a given start date.

Sounds simple enough, but this function lets you do some pretty useful things like calculating an estimated shipment date based on the ordered date.

## Differences in DATEADD syntax across data warehouse platforms[​](#differences-in-dateadd-syntax-across-data-warehouse-platforms "Direct link to Differences in DATEADD syntax across data warehouse platforms")

All of them accept the same rough parameters, in slightly different syntax and order:

* Start / from date
* Datepart (day, week, month, year)
* Interval (integer to increment by)

The *functions themselves* are named slightly differently, which is common across SQL dialects.

### For example, the DATEADD function in Snowflake…[​](#for-example-the-dateadd-function-in-snowflake "Direct link to For example, the DATEADD function in Snowflake…")

```text
dateadd( {{ datepart }}, {{ interval }}, {{ from_date }} )
```

*Hour, minute and second are supported!*

### The DATEADD function in Databricks[​](#the-dateadd-function-in-databricks "Direct link to The DATEADD function in Databricks")

```sql
date_add( {{ startDate }}, {{ numDays }} )
```

### The DATEADD function in BigQuery…[​](#the-dateadd-function-in-bigquery "Direct link to The DATEADD function in BigQuery…")

```sql
date_add( {{ from_date }}, INTERVAL {{ interval }} {{ datepart }} )
```

*Dateparts of less than a day (hour / minute / second) are not supported.*

### The DATEADD function in Postgres…[​](#the-dateadd-function-in-postgres "Direct link to The DATEADD function in Postgres…")

Postgres doesn’t provide a dateadd function out of the box, so you’ve got to go it alone - but the syntax looks very similar to BigQuery’s function…

```sql
{{ from_date }} + (interval '{{ interval }} {{ datepart }}')
```

Switching back and forth between those SQL syntaxes usually requires a quick scan through the warehouse’s docs to get back on the horse.

## Standardizing your DATEADD SQL syntax with a dbt macro[​](#standardizing-your-dateadd-sql-syntax-with-a-dbt-macro "Direct link to Standardizing your DATEADD SQL syntax with a dbt macro")

But couldn’t we be doing something better with those keystrokes, like typing out and then deleting a tweet?

dbt helps smooth out these wrinkles of writing [SQL across data warehouses](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros.md).

Instead of looking up the syntax each time you use it, you can just write it the same way each time, and the macro compiles it to run on your chosen warehouse:

```text
{{ dateadd(datepart, interval, from_date_or_timestamp) }}
```

Adding 1 month to a specific date would look like…

```text
{{ dateadd(datepart="month", interval=1, from_date_or_timestamp="'2021-08-12'") }}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
