# profile

dbt\_project.yml

```yml
profile: string
```

## Definition[​](#definition "Direct link to Definition")

The profile your dbt project should use to connect to your data warehouse.

* If you are developing in dbt: This configuration is not applicable
* If you are developing locally: This configuration is required, unless a command-line option like [`--profile`](https://docs.getdbt.com/docs/local/profiles.yml.md#overriding-profiles-and-targets) is supplied. The `--profile` flag overrides the profile set in `dbt_project.yml`.

## Related guides[​](#related-guides "Direct link to Related guides")

* [Connecting to your warehouse using the command line](https://docs.getdbt.com/docs/local/profiles.yml.md#connecting-to-your-warehouse-using-the-command-line)

## Recommendation[​](#recommendation "Direct link to Recommendation")

Often an organization has only one data warehouse, so it is sensible to use your organization's name as a profile name, in `snake_case`. For example:

* `profile: acme`
* `profile: jaffle_shop`

It is also reasonable to include the name of your warehouse technology in your profile name, particularly if you have multiple warehouses. For example:

* `profile: acme_snowflake`
* `profile: jaffle_shop_bigquery`
* `profile: jaffle_shop_redshift`
