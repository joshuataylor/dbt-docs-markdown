# Why can't I just write DML in my transformations?

#### `select` statements make transformations accessible[​](#select-statements-make-transformations-accessible "Direct link to select-statements-make-transformations-accessible")

More people know how to write `select` statements, than DML, making the transformation layer accessible to more people!

#### Writing good DML is hard[​](#writing-good-dml-is-hard "Direct link to Writing good DML is hard")

If you write the DDL / DML yourself you can end up getting yourself tangled in problems like:

* What happens if the table already exists? Or this table already exists as a view, but now I want it to be a table?
* What if the schema already exists? Or, should I check if the schema already exists?
* How do I replace a model atomically (such that there's no down-time for someone querying the table)
* What if I want to parameterize my schema so I can run these transformations in a development environment?
* What order do I need to run these statements in? If I run a `cascade` does it break other things?

Each of these problems *can* be solved, but they are unlikely to be the best use of your time.

#### dbt does more than generate SQL[​](#dbt-does-more-than-generate-sql "Direct link to dbt does more than generate SQL")

You can test your models, generate documentation, create snapshots, and more!

#### You reduce your vendor lock in[​](#you-reduce-your-vendor-lock-in "Direct link to You reduce your vendor lock in")

SQL dialects tend to diverge the most in DML and DDL (rather than in `select` statements) — check out the example [here](https://docs.getdbt.com/faqs/Models/sql-dialect.md). By writing less SQL, it can make a migration to a new database technology easier.

If you do need to write custom DML, there are ways to do this in dbt using [custom materializations](https://docs.getdbt.com/guides/create-new-materializations.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
