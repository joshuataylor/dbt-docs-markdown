# Which docs should I use when writing Jinja or creating a macro?

If you are stuck with a Jinja issue, it can get confusing where to check for more information. We recommend you check (in order):

1. [Jinja's Template Designer Docs](https://jinja.palletsprojects.com/page/templates/): This is the best reference for most of the Jinja you'll use
2. [Our Jinja function reference](https://docs.getdbt.com/reference/dbt-jinja-functions-context-variables.md): This documents any additional functionality we've added to Jinja in dbt.
3. [Agate's table docs](https://agate.readthedocs.io/page/api/table.html): If you're operating on the result of a query, dbt will pass it back to you as an agate table. This means that the methods you call on the table belong to the Agate library rather than Jinja or dbt.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
