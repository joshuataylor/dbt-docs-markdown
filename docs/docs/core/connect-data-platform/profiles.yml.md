# About profiles.yml

If you're using dbt from the command line, you need a `profiles.yml` file that contains the connection details for your data platform.

dbt platform accounts

dbt platform projects don't require a profiles.yml file unless you're developing from your local machine instead of the cloud-based UI.

## About profiles.yml[​](#about-profilesyml "Direct link to About profiles.yml")

The `profiles.yml` file stores database connection credentials and configuration for dbt projects, including:

* **Connection details** — Account identifiers, hosts, ports, and authentication credentials.
* **Target definitions** — Define different environments (dev, staging, prod) within a single profile.
* **Default target** — Set which environment to use by default.
* **Execution parameters** — Thread count, timeouts, and retry settings.
* **Credential separation** — Keep sensitive information out of version control.

The `profile` field in [`dbt_project.yml`](https://docs.getdbt.com/reference/dbt_project.yml.md) references a profile name defined in `profiles.yml`.

## Location of profiles.yml[​](#location-of-profilesyml "Direct link to Location of profiles.yml")

Only one `profiles.yml` file is required and it can manage multiple projects and connections.

* dbt Fusion
* dbt Core

Fusion searches for the parent directory of `profiles.yml` in the following order and uses the first location it finds:

1. `--profiles-dir` flag — Override for CI/CD or testing.
2. Project root directory — Project-specific credentials.
3. `~/.dbt/` directory (Recommended location) — Shared across all projects.

dbt Core searches for the parent directory of `profiles.yml` in the following order and uses the first location it finds:

1. `--profiles-dir` flag
2. `DBT_PROFILES_DIR` environment variable
3. Current working directory
4. `~/.dbt/` directory (Recommended location)

Note: dbt Core supports using the `DBT_PROFILES_DIR` environment variable or a `profiles.yml` file in the current working directory. These options aren't currently supported in Fusion.

`~/.dbt/profiles.yml` is the recommended location for the following reasons:

* **Security** — Keeps credentials out of project directories and version control.
* **Reusability** — A single file for all dbt projects on the machine.
* **Separation** — Connection details don't travel with project code.

#### When should I use project root?[​](#when-should-i-use-project-root "Direct link to When should I use project root?")

Place your `profiles.yml` file in the project root directory for:

* Self-contained demo or tutorial projects.
* Docker containers with baked-in credentials.
* CI/CD pipelines with environment-specific configs.

## Create and configure the `profiles.yml` file[​](#create-and-configure-the-profilesyml-file "Direct link to create-and-configure-the-profilesyml-file")

The easiest way to create and configure a `profiles.yml` file is to execute `dbt init` after you've installed dbt on your machine. This takes you through the process of configuring an adapter and places the file into the recommended `~/.dbt/` location.

If your project has an existing `profiles.yml` file, running `dbt init` will prompt you to amend or overwrite it. If you select the existing adapter for configuration, dbt will automatically populate the existing values.

You can also manually create the file and add it to the proper location. To configure an adapter manually, copy and paste the fields from the adapter setup instructions for [dbt Core](https://docs.getdbt.com/docs/core/connect-data-platform/about-core-connections.md) or [Fusion](https://docs.getdbt.com/docs/fusion/connect-data-platform-fusion/profiles.yml.md) along with the appropriate values for each.

### Example configuration[​](#example-configuration "Direct link to Example configuration")

The following example highlighs the format of the `profiles.yml` file. Note that many of the configs are adapter-specific and their syntax varies.

\~/.dbt/profiles.yml

```yml
my_project_profile:  # Profile name (matches dbt_project.yml)
  target: dev  # Default target to use
  outputs:
    dev: # Development environment
      type: adapter_type # Required: snowflake, bigquery, databricks, redshift, postgres, etc
      # Connection identifiers (placeholder examples, see adapter-specific pages for supported configs)
      account: abc123  
      database: docs_team 
      schema: dev_schema       
      # Authentication (adapter-specific)
      auth_method: username_password  
      username: username
      password_credentials: password
      # Execution settings (common across adapters)
      threads: 4   # Number of parallel threads

# Multiple profiles (for multiple projects)
my_second_project_profile:
  target: dev
  outputs:
    dev:
      type: snowflake  # Example adapter
      account: account
      user: user
      password: password
      database: database
      schema: schema
      warehouse: warehouse
      threads: 4
```

### Environment variables[​](#environment-variables "Direct link to Environment variables")

Use environment variables to keep sensitive credentials out of your `profiles.yml` file. Check out the [env\_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md) reference for more information.

Example:

\~/.dbt/profiles.yml

```yml
my_profile:
  target: dev
  outputs:
    dev:
      type: ADAPTER_NAME
      account: "{{ env_var("ADAPTER_ACCOUNT") }}"
      user: "{{ env_var("ADAPTER_USER") }}"
      password: "{{ env_var("ADAPTER_PASSWORD") }}"
      database: "{{ env_var("ADAPTER_DATABASE") }}"
      schema: "{{ env_var("ADAPTER_SCHEMA") }}"
      warehouse: "{{ env_var("ADAPTER_WAREHOUSE") }}"
      role: "{{ env_var("ADAPTER_ROLE") }}"
      threads: 4
```

## User config[​](#user-config "Direct link to User config")

You can set default values of global configs for all projects that you run using your local machine. Refer to [About global configs](https://docs.getdbt.com/reference/global-configs/about-global-configs.md) for details.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Install dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion-install.md)
* [Connection profiles](https://docs.getdbt.com/docs/fusion/connect-data-platform-fusion/connection-profiles.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
