# Does my operating system have prerequisites?

Your operating system may require pre-installation setup before installing dbt Core with pip. After downloading and installing any dependencies specific to your development environment, you can proceed with the [pip installation of dbt Core](https://docs.getdbt.com/docs/core/pip-install.md).

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
