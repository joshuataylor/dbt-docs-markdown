# About dbt environment command

info

The dbt platform CLI provides the `dbt environment` command for environment and connection details. If you're using Fusion or dbt Core, use `dbt debug` to inspect profile, target, and connection — or use `dbtf debug` if you have more than one dbt CLI and want to inspect Fusion.

The `dbt environment` command enables you to interact with your dbt environment. Use the command for:

* Viewing your local configuration details (account ID, active project ID, deployment environment, and more).
* Viewing your dbt configuration details (environment ID, environment name, connection type, and more).

This guide lists all the commands and options you can use with `dbt environment` in the [dbt CLI](https://docs.getdbt.com/docs/cloud/cloud-cli-installation.md). To use them, add a command or option like this: `dbt environment [command]` or use the shorthand `dbt env [command]`.

### dbt environment show[​](#dbt-environment-show "Direct link to dbt environment show")

The `show` command allows you to view your local and dbt configuration details. To run the command with the dbt CLI, enter one of the following commands, including the shorthand:

```shell
dbt environment show
```

```shell
dbt env show
```

The command returns the following information:

```bash
❯ dbt env show
Local Configuration:
  Active account ID              185854
  Active project ID              271692
  Active host name               cloud.getdbt.com
  dbt_cloud.yml file path        /Users/cesar/.dbt/dbt_cloud.yml
  dbt_project.yml file path      /Users/cesar/git/cloud-cli-test-project/dbt_project.yml
  <Constant name="cloud" /> CLI version          0.35.7
  OS info                        darwin arm64

Cloud Configuration:
  Account ID                     185854
  Project ID                     271692
  Project name                   Snowflake
  Environment ID                 243762
  Environment name               Development
  Defer environment ID           [N/A]
  dbt version                    1.6.0-latest
  Target name                    default
  Connection type                snowflake

Snowflake Connection Details:
  Account                        ska67070
  Warehouse                      DBT_TESTING_ALT
  Database                       DBT_TEST
  Schema                         CLOUD_CLI_TESTING
  Role                           SYSADMIN
  User                           dbt_cloud_user
  Client session keep alive      false 
```

Note, that dbt won't return anything that is a secret key and will return an 'NA' for any field that isn't configured.

### dbt environment flags[​](#dbt-environment-flags "Direct link to dbt environment flags")

Use the following flags (or options) with the `dbt environment` command:

* `-h`, `--help` — To view the help documentation for a specific command in your command line interface.

  ```shell
  dbt environment [command] --help
  ```

  The `--help` flag returns the following information:

  ```bash
    ❯ dbt help environment
    Interact with dbt environments

  Usage:
    dbt environment [command]

  Aliases:
    environment, env

  Available Commands:
    show        Show the working environment

  Flags:
    -h, --help   help for environment

  Use "dbt environment [command] --help" for more information about a command.
  ```

  For example, to view the help documentation for the `show` command, enter one of the following commands, including the shorthand:

  ```shell
  dbt environment show --help
  dbt env show -h
  ```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
