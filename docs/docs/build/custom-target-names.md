# Custom target names

## dbt Scheduler[​](#dbt-scheduler "Direct link to dbt Scheduler")

You can define a custom target name for any dbt job to correspond to settings in your dbt project. This is helpful if you have logic in your dbt project that behaves differently depending on the specified target, for example:

```sql
select *
from a_big_table

-- limit the amount of data queried in dev
{% if target.name != 'prod' %}
where created_at > date_trunc('month', current_date)
{% endif %}
```

To set a custom target name for a job in dbt, configure the **Target Name** field for your job in the Job Settings page.

[![Overriding the target name to 'prod'](/img/docs/dbt-cloud/using-dbt-cloud/jobs-settings-target-name.png?v=2 "Overriding the target name to 'prod'")](#)Overriding the target name to 'prod'

## dbt Studio IDE[​](#dbt-studio-ide "Direct link to dbt Studio IDE")

When developing in dbt, you can set a custom target name in your development credentials. Click your account name above the profile icon in the left panel, select **Account settings**, then go to **Credentials**. Choose the project to update the target name.

[![Overriding the target name to 'dev'](/img/docs/dbt-cloud/using-dbt-cloud/development-credentials.png?v=2 "Overriding the target name to 'dev'")](#)Overriding the target name to 'dev'

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
