# Dremio setup

* **Maintained by**:
  <!-- -->
  Dremio
* **Authors**:
  <!-- -->
  Dremio
* **GitHub repo**: [dremio/dbt-dremio](https://github.com/dremio/dbt-dremio) [![](https://img.shields.io/github/stars/dremio/dbt-dremio?style=for-the-badge)](https://github.com/dremio/dbt-dremio)
* **PyPI package**: `dbt-dremio` [![](https://badge.fury.io/py/dbt-dremio.svg)](https://badge.fury.io/py/dbt-dremio)
* **Slack channel**: [db-dremio](https://docs.getdbt.com/\[https://www.getdbt.com/community]\(https://getdbt.slack.com/archives/C049G61TKBK\))
* **Supported dbt Core version**:
  <!-- -->
  v1.8.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  Dremio 22.0

## Installing <!-- -->dbt-dremio

Use `pip` to install the adapter. Before 1.8, installing the adapter would automatically install `dbt-core` and any additional dependencies. Beginning in 1.8, installing an adapter does not automatically install `dbt-core`. This is because adapters and dbt Core versions have been decoupled from each other so we no longer want to overwrite existing dbt-core installations. Use the following command for installation:

`python -m pip install dbt-core dbt-dremio`

## Configuring <!-- -->dbt-dremio<!-- -->

For <!-- -->Dremio<!-- -->-specific configuration, please refer to [Dremio<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/no-configs.md)

Follow the repository's link for OS dependencies.

note

[Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md) are not supported.

## Prerequisites for Dremio Cloud[​](#prerequisites-for-dremio-cloud "Direct link to Prerequisites for Dremio Cloud")

Before connecting from project to Dremio Cloud, follow these prerequisite steps:

* Ensure that you have the ID of the Sonar project that you want to use. See [Obtaining the ID of a Project](https://docs.dremio.com/cloud/cloud-entities/projects/#obtaining-the-id-of-a-project).
* Ensure that you have a personal access token (PAT) for authenticating to Dremio Cloud. See [Creating a Token](https://docs.dremio.com/cloud/security/authentication/personal-access-token/#creating-a-token).
* Ensure that Python 3.9.x or later is installed on the system that you are running dbt on.

## Prerequisites for Dremio Software[​](#prerequisites-for-dremio-software "Direct link to Prerequisites for Dremio Software")

* Ensure that you are using version 22.0 or later.

* Ensure that Python 3.9.x or later is installed on the system that you are running dbt on.

* If you want to use TLS to secure the connection between dbt and Dremio Software, configure full wire encryption in your Dremio cluster. For instructions, see [Configuring Wire Encryption](https://docs.dremio.com/software/deployment/wire-encryption-config/).

## Initializing a Project[​](#initializing-a-project "Direct link to Initializing a Project")

1. Run the command `dbt init <project_name>`.

2. Select `dremio` as the database to use.

3. Select one of these options to generate a profile for your project:

   <!-- -->

   * `dremio_cloud` for working with Dremio Cloud
   * `software_with_username_password` for working with a Dremio Software cluster and authenticating to the cluster with a username and a password
   * `software_with_pat` for working with a Dremio Software cluster and authenticating to the cluster with a personal access token

Next, configure the profile for your project.

## Profiles[​](#profiles "Direct link to Profiles")

When you initialize a project, you create one of these three profiles. You must configure it before trying to connect to Dremio Cloud or Dremio Software.

* Profile for Dremio Cloud
* Profile for Dremio Software with Username/Password Authentication
* Profile for Dremio Software with Authentication Through a Personal Access Token

For descriptions of the configurations in these profiles, see [Configurations](#configurations).

* Cloud
* Software (Username/Password)
* Software (Personal Access Token)

```yaml
[project name]:
  outputs:
    dev:
      cloud_host: api.dremio.cloud
      cloud_project_id: [project ID]
      object_storage_source: [name]
      object_storage_path: [path]
      dremio_space: [name]
      dremio_space_folder: [path]
      pat: [personal access token]
      threads: [integer >= 1]
      type: dremio
      use_ssl: true
      user: [email address]
  target: dev
```

```yaml
[project name]:
  outputs:
    dev:
      password: [password]
      port: [port]
      software_host: [hostname or IP address]
      object_storage_source: [name
      object_storage_path: [path]
      dremio_space: [name]
      dremio_space_folder: [path]
      threads: [integer >= 1]
      type: dremio
      use_ssl: [true|false]
      user: [username]
  target: dev
```

```yaml
[project name]:
  outputs:
    dev:
      pat: [personal access token]
      port: [port]
      software_host: [hostname or IP address]
      object_storage_source: [name
      object_storage_path: [path]
      dremio_space: [name]
      dremio_space_folder: [path]
      threads: [integer >= 1]
      type: dremio
      use_ssl: [true|false]
      user: [username]
  target: dev
```

## Configurations Common to Profiles for Dremio Cloud and Dremio Software[​](#configurations-common-to-profiles-for-dremio-cloud-and-dremio-software "Direct link to Configurations Common to Profiles for Dremio Cloud and Dremio Software")

| Configuration           | Required? | Default Value  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------- | --------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`                  | Yes       | dremio         | Auto-populated when creating a Dremio project. Do not change this value.                                                                                                                                                                                                                                                                                                                                                                          |
| `threads`               | Yes       | 1              | The number of threads the dbt project runs on.                                                                                                                                                                                                                                                                                                                                                                                                    |
| `object_storage_source` | No        | $scratch       | The name of the filesystem in which to create tables, materialized views, tests, and other objects. The dbt alias is `datalake`. This name corresponds to the name of a source in the **Object Storage** section of the Datasets page in Dremio, which is "Samples" in the following image: ![dbt samples path](/assets/images/dbt-Samples-91e0c8e2d2bf58047d38b76202049e8a.png)                                                                  |
| `object_storage_path`   | No        | `no_schema`    | The path in the filesystem in which to create objects. The default is the root level of the filesystem. The dbt alias is `root_path`. Nested folders in the path are separated with periods. This value corresponds to the path in this location in the Datasets page in Dremio, which is "samples.dremio.com.Dremio University" in the following image: ![dbt samples path](/assets/images/dbt-SamplesPath-b7c23597a4bbb05010de7a9a1d938da4.png) |
| `dremio_space`          | No        | `@\<username>` | The value of the Dremio space in which to create views. The dbt alias is `database`. This value corresponds to the name in this location in the **Spaces** section of the Datasets page in Dremio: ![dbt spaces](/assets/images/dbt-Spaces-3b4197940bedfd0a179d74eef56df501.png)                                                                                                                                                                  |
| `dremio_space_folder`   | No        | `no_schema`    | The folder in the Dremio space in which to create views. The default is the top level in the space. The dbt alias is `schema`. Nested folders are separated with periods. This value corresponds to the path in this location in the Datasets page in Dremio, which is `Folder1.Folder2` in the following image: ![Folder1.Folder2](/assets/images/dbt-SpacesPath-6f785309a289a80a338c03c0bf2879ea.png)                                           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Configurations in Profiles for Dremio Cloud[​](#configurations-in-profiles-for-dremio-cloud "Direct link to Configurations in Profiles for Dremio Cloud")

| Configuration      | Required? | Default Value      | Description                                                                                                                                                                                               |
| ------------------ | --------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `cloud_host`       | Yes       | `api.dremio.cloud` | US Control Plane: `api.dremio.cloud`<br />EU Control Plane: `api.eu.dremio.cloud`                                                                                                                         |
| `user`             | Yes       | None               | Email address used as a username in Dremio Cloud                                                                                                                                                          |
| `pat`              | Yes       | None               | The personal access token to use for authentication. See [Personal Access Tokens](https://docs.dremio.com/cloud/security/authentication/personal-access-token/) for instructions about obtaining a token. |
| `cloud_project_id` | Yes       | None               | The ID of the Sonar project in which to run transformations.                                                                                                                                              |
| `use_ssl`          | Yes       | `true`             | The value must be `true`.                                                                                                                                                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Configurations in Profiles for Dremio Software[​](#configurations-in-profiles-for-dremio-software "Direct link to Configurations in Profiles for Dremio Software")

| Configuration   | Required?                                                       | Default Value | Description                                                                                                                                                                                                                                                                                                                                |
| --------------- | --------------------------------------------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `software_host` | Yes                                                             | None          | The hostname or IP address of the coordinator node of the Dremio cluster.                                                                                                                                                                                                                                                                  |
| `port`          | Yes                                                             | `9047`        | Port for Dremio Software cluster API endpoints.                                                                                                                                                                                                                                                                                            |
| `user`          | Yes                                                             | None          | The username of the account to use when logging into the Dremio cluster.                                                                                                                                                                                                                                                                   |
| `password`      | Yes, if you are not using the pat configuration.                | None          | The password of the account to use when logging into the Dremio cluster.                                                                                                                                                                                                                                                                   |
| `pat`           | Yes, if you are not using the user and password configurations. | None          | The personal access token to use for authenticating to Dremio. See [Personal Access Tokens](https://docs.dremio.com/software/security/personal-access-tokens/) for instructions about obtaining a token. The use of a personal access token takes precedence if values for the three configurations user, password, and pat are specified. |
| `use_ssl`       | Yes                                                             | `true`        | Acceptable values are `true` and `false`. If the value is set to true, ensure that full wire encryption is configured in your Dremio cluster. See [Prerequisites for Dremio Software](#prerequisites-for-dremio-software).                                                                                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
