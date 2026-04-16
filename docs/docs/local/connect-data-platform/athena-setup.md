# Connect Athena to dbt Core


<!--The following code uses a component and the built-in docusaurus markdown partials file, which contains reusable content assigned in the meta frontmatter. For this page, the partial file is _setup-pages-intro.md. You have to include the 'import' code and then assign the component as needed.  -->

import SetUpPages from '/snippets/_setup-pages-intro.md';

<SetUpPages meta={frontMatter.meta} />

:::tip `dbt-athena` vs `dbt-athena-community`

`dbt-athena-community` was the community-maintained adapter until dbt Labs took over maintenance in late 2024. Both `dbt-athena` and `dbt-athena-community` are maintained by dbt Labs, but `dbt-athena-community` is now simply a wrapper on `dbt-athena`, published for backwards compatibility.

:::

## Connecting to Athena with dbt-athena

This plugin does not accept any credentials directly. Instead, [credentials are determined automatically](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html) based on AWS CLI/boto3 conventions and stored login info. You can configure the AWS profile name to use via `aws_profile_name`. Alternatively, you can set explicit credential keys in the profile: `aws_access_key_id`, `aws_secret_access_key`, and, for [AWS STS temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html), `aws_session_token` (must match exactly). 

<File name='~/.dbt/profiles.yml'>

```yaml
default:
  outputs:
    dev:
      type: athena
      s3_staging_dir: [s3_staging_dir]
      s3_data_dir: [s3_data_dir]
      s3_data_naming: [table_unique] # the type of naming convention used when writing to S3
      region_name: [region_name]
      database: [database name]
      schema: [dev_schema]
      aws_profile_name: [optional profile to use from your AWS shared credentials file.]
      threads: [1 or more]
      num_retries: [0 or more] # number of retries performed by the adapter. Defaults to 5
  target: dev
```

</File>

### Example Config

<File name='profiles.yml'>

```yaml
default:
  outputs:
    dev:
      type: athena
      s3_staging_dir: s3://dbt_demo_bucket/athena-staging/
      s3_data_dir: s3://dbt_demo_bucket/dbt-data/
      s3_data_naming: schema_table 
      region_name: us-east-1
      database: warehouse 
      schema: dev
      aws_profile_name: demo
      threads: 4 
      num_retries: 3    
  target: dev
```

</File>
