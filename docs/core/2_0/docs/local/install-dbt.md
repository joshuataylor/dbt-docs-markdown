# Install dbt locally

Get dbt running on your machine in a few minutes. Choose your path:

* Fusion + dbt extension (recommended)
* dbt Core v2
* dbt Core v1

## Install the Fusion CLI [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#install-the-fusion-cli- "Direct link to install-the-fusion-cli-")

The Fusion CLI delivers dbt Fusion engine performance benefits (faster parsing, compilation, and execution) but doesn't include LSP features. For the best dbt Fusion engine experience, install the dbt VS Code extension in VS Code or a compatible IDE.

Choose your preferred installation method:

 Pip installation for Windows, macOS, and Linux

```shell
python -m pip install --pre dbt
```

To upgrade to a newer version:

```shell
python -m pip install --upgrade --pre dbt
```

 CDN installation for macOS and Linux

```shell
curl -fsSL https://public.cdn.getdbt.com/fs/install/install.sh | sh -s -- --update
```

To use `dbt` immediately after installation, close and reopen your terminal or reload your shell so that the new `$PATH` is recognized:

```shell
exec $SHELL
```

To upgrade to a newer version:

```shell
dbt system update
```

note

`dbtf system update` installs Fusion globally — it updates your `PATH` in `~/.zshrc` and creates a `dbtf` alias. To manage multiple versions or isolate your install, use separate shell profiles or virtual environments.

 CDN installation for Windows

```powershell
irm https://public.cdn.getdbt.com/fs/install/install.ps1 | iex
```

To use `dbt` immediately after installation, close and reopen or reload your shell so that the new `Path` is recognized:

```powershell
Start-Process powershell
```

To upgrade to a newer version:

```shell
dbt system update
```

 Homebrew installation for macOS

```shell
brew install dbt
```

To upgrade to a newer version:

```shell
brew upgrade dbt
```

 Winget installation for Windows

```shell
winget install --id dbtLabs.dbt --exact
```

To upgrade to a specific version:

```shell
winget install --id dbtLabs.dbt --exact --version <version>
```

Run the following command to verify your installation:

```bash
dbt --version
```

You can use `dbt` or its Fusion alias `dbtf` (handy if you already have the Core or platform CLI installed). Default install path:

* macOS/Linux: `$HOME/.local/bin/dbt`
* Windows: `C:\Users\<username>\.local\bin\dbt.exe`

The installer adds this path automatically, but you may need to reload your shell for the `dbtf` command to work.

## Install the extension[​](#install-the-extension "Direct link to Install the extension")

Install the dbt VS Code extension from the marketplace for [VS Code and Cursor](https://marketplace.visualstudio.com/items?itemName=dbtLabsInc.dbt\&ssr=false#overview) or [Windsurf](https://open-vsx.org/extension/dbtLabsInc/dbt).

1. In your editor, open the **Extensions** tab and search for `dbt`.

2. Locate the extension from the publisher `dbtLabsInc` or `dbt Labs Inc`, then click **Install**.

   [![Search for the extension](/img/docs/extension/extension-marketplace.png?v=2 "Search for the extension")](#)Search for the extension

3. Confirm that the extension is active by checking for the **dbt Extension** label in the status bar. Hover over the label to view diagnostic information.

   [![If you see the 'dbt Extension' label, the extension is activated](/img/docs/extension/dbt-extension-statusbar.png?v=2 "If you see the 'dbt Extension' label, the extension is activated")](#)If you see the 'dbt Extension' label, the extension is activated

4. After the extension activates, it automatically downloads the correct dbt Language Server (LSP) for your operating system.

   [![The dbt Language Server will be installed automatically](/img/docs/extension/extension-lsp-download.png?v=2 "The dbt Language Server will be installed automatically")](#)The dbt Language Server will be installed automatically

Refer to the [dbt VS Code extension docs](https://docs.getdbt.com/docs/about-dbt-extension.md) for more information.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

Common issues and resolutions:

* **dbt command not found:** Add the installation location to your `$PATH`.
* **Version conflicts:** Check that no other dbt Core or dbt CLI versions are installed or active on your machine.
* **Installation permissions:** Make sure your user account can install software locally.

## Frequently asked questions[​](#frequently-asked-questions "Direct link to Frequently asked questions")

* Can I revert to my previous dbt installation?

  Yes. To test Fusion without affecting your existing workflows, use a separate environment or virtual machine.

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)

## Install dbt Core v2 CLI Alpha[​](#install-dbt-core-v2-cli- "Direct link to install-dbt-core-v2-cli-")

dbt Core v2 is in alpha

dbt Core v2 is under active development and not recommended for production use. Features and APIs may change before the stable release. For stable local development, use Fusion.

dbt Core v2 is the next major version of dbt Core, built on the dbt Fusion engine runtime. Install it with `pip`, same as v1, but target the v2 prerelease package.

 Pip installation

```shell
python -m pip install --pre dbt-core
```

```shell
dbt --version
```

Confirm that the installed version begins with `2.`.

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)

## Install dbt Core v1 CLI[​](#install-dbt-core-v1-cli "Direct link to Install dbt Core v1 CLI")

dbt Core v1 is the original open-source dbt engine. Install it with `pip`, Docker, or from source.

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
