# MindsDB configurations

## Authentication[​](#authentication "Direct link to Authentication")

To succesfully connect dbt to MindsDB, you will need to provide the following configuration from the MindsDB instance.

| Key      | Required | Description                                          | Self-hosted                                                                    | MindsDB Cloud                                                                  |
| -------- | -------- | ---------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| type     | ✔️       | The specific adapter to use                          | `mindsdb`                                                                      | `mindsdb`                                                                      |
| host     | ✔️       | The MindsDB (hostname) to connect to                 | Default to `172.0.0.1`                                                         | Default to `cloud.mindsdb.com`                                                 |
| port     | ✔️       | The port to use                                      | Default to `47335`                                                             | Default to `3306`                                                              |
| schema   | ✔️       | Specify the schema (database) to build models into   | The MindsDB [integration name](https://docs.mindsdb.com/sql/create/databases/) | The MindsDB [integration name](https://docs.mindsdb.com/sql/create/databases/) |
| username | ✔️       | The username to use to connect to the server         | Default to `mindsdb`                                                           | Your mindsdb cloud username                                                    |
| password | ✔️       | The password to use for authenticating to the server | No password by default                                                         | Your mindsdb cloud password                                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Usage[​](#usage "Direct link to Usage")

Create dbt project, choose mindsdb as the database and set up the connection. Verify your connection works `dbt debug`

`dbt init <project_name>`

To create a predictor, create a dbt model with a "predictor" materialization. The name of the model will be the name of predictor.

#### Parameters:[​](#parameters "Direct link to Parameters:")

* `integration` - name of used integration to get data from and save result to. Must be created in mindsdb beforehand using the [`CREATE DATABASE` syntax](https://docs.mindsdb.com/sql/create/databases/).
* `predict` - field for prediction
* `predict_alias` \[optional] - alias for predicted field
* `using` \[optional] - options for configure trained model

```sql
-- my_first_model.sql    
    {{
        config(
            materialized='predictor',
            integration='photorep',
            predict='name',
            predict_alias='name',
            using={
                'encoders.location.module': 'CategoricalAutoEncoder',
                'encoders.rental_price.module': 'NumericEncoder'
            }
        )
    }}
      select * from stores
```

To apply predictor add dbt model with "table" materialization. It creates or replaces table in selected integration with results of predictor. Name of the model is used as name of the table to store prediction results. If you need to specify schema you can do it with dot separator: schema\_name.table\_name.sql

#### Parameters[​](#parameters-1 "Direct link to Parameters")

* `predictor_name` - name of the predictor. It has to be created in mindsdb.
* `integration` - name of used integration to get data from and save result to. Must be created in mindsdb beforehand using the [`CREATE DATABASE` syntax](https://docs.mindsdb.com/sql/create/databases/).

```text
    {{ config(materialized='table', predictor_name='TEST_PREDICTOR_NAME', integration='photorep') }}
        select a, bc from ddd where name > latest
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
