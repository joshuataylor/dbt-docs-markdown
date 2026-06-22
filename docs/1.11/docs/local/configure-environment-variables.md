# Configure environment variables

Fusion automatically loads environment variables from a `.env` file in your current working directory (the folder you `cd` into and run dbt commands from in your terminal). This helps you manage credentials and settings without hardcoding them in your `profiles.yml` or exposing them in your shell history.

## Using a `.env` file[​](#using-a-env-file "Direct link to using-a-env-file")

1. Create a `.env` file in your current working directory (typically at the root of your dbt project):

   ```env
   DBT_MY_DATABASE=my_database
   DBT_MY_SCHEMA=my_schema
   DBT_SECRET_KEY=my_secret_value
   ```

2. Reference these variables in your `profiles.yml` using the [`env_var` Jinja function](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md):

   ```yaml
   my_profile:
     target: dev
     outputs:
       dev:
         type: snowflake
         account: my_account
         database: "{{ env_var('DBT_MY_DATABASE') }}"
         schema: "{{ env_var('DBT_MY_SCHEMA') }}"
   ```

3. Run dbt commands normally. Fusion will automatically load the variables from the `.env` file. For example, running `dbtf debug` will show your connection using the values from `.env`:

   ```shell
   dbtf debug
   ...
   Debugging connection:
   "authenticator": "my_authenticator",
   "account": "my_account",
   "user": "my_user",
   "database": "my_database",        # Loaded from DBT_MY_DATABASE in .env
   "schema": "my_schema",            # Loaded from DBT_MY_SCHEMA in .env
   ```

note

We recommend placing your `.env` file in the project root and running dbt commands from that location because the file is loaded *only* from your current working directory. It doesn't support the `--project-dir` flag or `DBT_ENGINE_PROJECT_DIR` environment variable, and dbt won't search your project root if you're running commands from a different directory location.

### Precedence order[​](#precedence-order "Direct link to Precedence order")

When the same environment variable is defined in multiple places, Fusion uses the following precedence order (highest to lowest):

1. Shell environment — Variables set directly in your shell (for example, `export DBT_MY_VAR=value`)
2. `.env` file — Variables defined in the `.env` file in your current working directory

This means environment variables set in your shell always override values from the `.env` file.

tip

Add `.env` to your `.gitignore` file to prevent sensitive credentials from being committed to version control. The `dbtf init` command automatically includes `.env` in the generated `.gitignore` file.

For more details on managing environment variables locally, refer to [Configure your local environment](https://docs.getdbt.com/docs/configure-dbt-extension.md#set-environment-variables-locally).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
