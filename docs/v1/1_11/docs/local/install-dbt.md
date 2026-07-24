# Install dbt

Local developmentⓘ

Want faster dbt?

Upgrade to v2 to get Fusion — up to 30x faster performance, LSP features like autocomplete and inline errors, and more. [Upgrade to v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md).

## Install dbt Core v1.x CLI[​](#install-dbt-core-v1x-cli "Direct link to Install dbt Core v1.x CLI")

dbt Core v1.x is the original open-source dbt engine. Install it with `pip`, Docker, or from source.

 Pip installation

### Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [Python](https://www.python.org/downloads/) (`python --version` or `python3 --version`)
* [pip](https://pip.pypa.io/en/stable/installation/) (`pip --version` or `pip3 --version`)

Does my operating system have prerequisites?

Your operating system may require pre-installation setup before installing dbt Core with pip. After downloading and installing any dependencies specific to your development environment, you can proceed with the [pip installation of dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md).

### CentOS[​](#centos "Direct link to CentOS")

CentOS requires Python and some other dependencies to successfully install and run dbt Core.

To install Python and other dependencies:

```shell

sudo yum install redhat-rpm-config gcc libffi-devel \
  python-devel openssl-devel
```

### MacOS[​](#macos "Direct link to MacOS")

The MacOS requires Python 3.8 or higher to successfully install and run dbt Core.

To check the Python version:

```shell

python --version
```

If you need a compatible version, you can download and install [Python version 3.9 or higher for MacOS](https://www.python.org/downloads/macos).

If your machine runs on an Apple M1 architecture, we recommend that you install dbt via [Rosetta](https://support.apple.com/en-us/HT211861). This is necessary for certain dependencies that are only supported on Intel processors.

### Ubuntu/Debian[​](#ubuntudebian "Direct link to Ubuntu/Debian")

Ubuntu requires Python and other dependencies to successfully install and run dbt Core.

To install Python and other dependencies:

```shell

sudo apt-get install git libpq-dev python-dev python3-pip
sudo apt-get remove python-cffi
sudo pip install --upgrade cffi
pip install cryptography~=3.4
```

### Windows[​](#windows "Direct link to Windows")

Windows requires Python and git to successfully install and run dbt Core.

Install [Git for Windows](https://git-scm.com/downloads) and [Python version 3.9 or higher for Windows](https://www.python.org/downloads/windows/).

For further questions, please see the [Python compatibility FAQ](https://docs.getdbt.com/faqs/Core/install-python-compatibility.md)

What version of Python can I use?

Use this table to match dbt Core versions with their compatible Python versions. New [dbt minor versions](https://docs.getdbt.com/docs/dbt-versions.md#minor-versions) will add support for new Python3 minor versions when all dependencies can support it. In addition, dbt minor versions will withdraw support for old Python3 minor versions before their [end of life](https://endoflife.date/python).

## Python compatibility matrix[​](#python-compatibility-matrix "Direct link to Python compatibility matrix")

| dbt-core version | v1.12 | v1.11 | v1.10 | v1.9 | v1.8 | v1.7 | v1.6 | v1.5 | v1.4 | v1.3 | v1.2 | v1.1 | v1.0 |
| ---------------- | ----- | ----- | ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Python 3.14      | ✅    | ❌    | ❌    | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   |
| Python 3.13      | ✅    | ✅    | ⚠️    | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   |
| Python 3.12      | ✅    | ✅    | ✅    | ✅   | ✅   | ✅   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   |
| Python 3.11      | ✅    | ✅    | ✅    | ✅   | ✅   | ✅   | ✅   | ✅   | ✅   | ❌   | ❌   | ❌   | ❌   |
| Python 3.10      | ✅    | ✅    | ✅    | ✅   | ✅   | ✅   | ✅   | ✅   | ✅   | ✅   | ✅   | ✅   | ✅   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

⚠️ Python 3.13 is supported in dbt Core v1.10 for the Postgres adapter.

Adapter plugins and their dependencies are not always compatible with the latest version of Python.

Note that this shouldn't be confused with [dbt Python models](https://docs.getdbt.com/docs/build/python-models.md#specific-data-platforms). If you're using a data platform that supports Snowpark, use the `python_version` config to run a Snowpark model with [Python versions](https://docs.snowflake.com/en/developer-guide/snowpark/python/setup) 3.9, 3.10, or 3.11.

### Create a virtual environment[​](#create-a-virtual-environment "Direct link to Create a virtual environment")

* Unix/macOS
* Windows

```shell
python3 -m venv .venv
source .venv/bin/activate
```

```shell
py -m venv .venv
.venv\Scripts\activate
```

To deactivate, run `deactivate`. To auto-activate in your new shell sessions, add an alias to your `~/.bashrc` or `~/.zshrc`:

```shell
alias env_dbt='source <PATH_TO_VIRTUAL_ENV_CONFIG>/bin/activate'
```

### Install your adapter[​](#install-your-adapter "Direct link to Install your adapter")

Installing an adapter automatically installs `dbt-core`. Choose your adapter from [Supported Data Platforms](https://docs.getdbt.com/docs/supported-data-platforms.md):

```shell
python -m pip install dbt-ADAPTER_NAME
```

To install `dbt-core` without an adapter (for tool integrations only):

```shell
python -m pip install dbt-core
```

### Upgrade[​](#upgrade "Direct link to Upgrade")

```shell
# Upgrade adapter (and dbt-core)
python -m pip install --upgrade dbt-ADAPTER_NAME

# Downgrade to a specific version
python -m pip install --upgrade dbt-core==1.9
```

### Install a prerelease[​](#install-a-prerelease "Direct link to Install a prerelease")

Use `--pre` to install prerelease versions. This may also install prerelease versions of other dependencies.

```shell
python3 -m pip install --pre dbt-ADAPTER_NAME
```

 Docker

dbt Core images are distributed via [GitHub Packages](https://github.com/dbt-labs/dbt-core/pkgs/container/dbt-core) and include pinned versions of dbt-core, one or more adapters, and all dependencies.

### Prerequisites[​](#prerequisites-1 "Direct link to Prerequisites")

* [Docker](https://docs.docker.com/) installed
* Familiarity with [adapters](https://docs.getdbt.com/docs/supported-data-platforms.md) and [Core versioning](https://docs.getdbt.com/docs/dbt-versions.md)

### Pull an image[​](#pull-an-image "Direct link to Pull an image")

Images follow the pattern `ghcr.io/dbt-labs/<db_adapter_name>:<version_tag>`. Available tags:

* `latest` — latest overall release
* `<Major>.<Minor>.latest` — latest patch for a version family (for example, `1.9.latest`)

```shell
docker pull ghcr.io/dbt-labs/<db_adapter_name>:<version_tag>
```

### Run dbt in a container[​](#run-dbt-in-a-container "Direct link to Run dbt in a container")

Bind-mount your project and profiles, then run any dbt command:

```shell
docker run \
--network=host \
--mount type=bind,source=/absolute/path/to/project,target=/usr/app \
--mount type=bind,source=/absolute/path/to/profiles.yml,target=/root/.dbt/profiles.yml \
<dbt_image_name> \
<dbt_command>
```

Note: bind-mount sources must be absolute paths. You may need to adjust `--network` settings depending on your warehouse host.

### Build a custom image[​](#build-a-custom-image "Direct link to Build a custom image")

If the pre-made images don't fit your use case, use the [`Dockerfile`](https://github.com/dbt-labs/dbt-core/blob/1.latest/docker/Dockerfile) and [`README`](https://github.com/dbt-labs/dbt-core/blob/1.latest/docker/README.md) to build images with multiple adapters, third-party adapters, or different system architectures. Custom image builds are community-supported — [open an issue](https://github.com/dbt-labs/dbt-core/issues) or [ask the community](https://docs.getdbt.com/community/resources/getting-help.md) if you run into trouble.

 Source

Install from source to get unreleased code or a specific commit. Clone the repo and install with `pip`:

```shell
git clone https://github.com/dbt-labs/dbt-core.git
cd dbt-core
python -m pip install -r requirements.txt
```

For editable mode (changes take effect immediately):

```shell
python -m pip install -e editable-requirements.txt
```

### Install an adapter from source[​](#install-an-adapter-from-source "Direct link to Install an adapter from source")

Install `dbt-core` first, then clone and install your adapter. For example, for Redshift:

```shell
git clone https://github.com/dbt-labs/dbt-redshift.git
cd dbt-redshift
python -m pip install .
```

For editable mode: `python -m pip install -e .`

For more details, read the [contributing guidelines](https://github.com/dbt-labs/dbt-core/blob/HEAD/CONTRIBUTING.md).

Pro tip: Using the --help flag

Most command-line tools, including dbt, support a `--help` flag that shows available commands and arguments. With dbt, you can use `--help` in two ways:<br /><br />— `dbt --help`: Shows available dbt commands<br />— `dbt run --help`: Shows available flags for the `run` command

## Next steps[​](#next-steps "Direct link to Next steps")

* Configure [environment variables](https://docs.getdbt.com/docs/local/configure-environment-variables.md) to manage credentials.
* Configure your [profiles.yml](https://docs.getdbt.com/docs/local/profiles.yml.md#location-of-profilesyml) file.
* Configure your [data platform connection](https://docs.getdbt.com/docs/local/connect-data-platform/about-dbt-connections.md).
* Create your first [dbt project](https://docs.getdbt.com/docs/build/projects.md) using the [`dbt init`](https://docs.getdbt.com/reference/commands/init.md) command.
