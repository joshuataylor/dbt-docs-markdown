# About run\_started\_at variable

`run_started_at` outputs the timestamp that this run started, e.g. `2017-04-21 01:23:45.678`.

The `run_started_at` variable is a Python `datetime` object. As of 0.9.1, the timezone of this variable defaults to UTC.

run\_started\_at\_example.sql

```sql
select
	'{{ run_started_at.strftime("%Y-%m-%d") }}' as date_day

from ...
```

To modify the timezone of this variable, use the `pytz` module:

run\_started\_at\_utc.sql

```sql
select
	'{{ run_started_at.astimezone(modules.pytz.timezone("America/New_York")) }}' as run_started_est

from ...
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
