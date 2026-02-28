# What are the best practices for installing dbt Core with pip?

info

The dbt Fusion engine is a next-generation, Rust-based engine that powers dbt development across the platform and local tooling. See [dbt Fusion engine](https://docs.getdbt.com/docs/fusion.md) for more information.

## Best practices[​](#best-practices "Direct link to Best practices")

Managing Python local environments can be challenging! You can use these best practices to improve the dbt Core installation with `pip`.

| Best practice                                                                                                                         | Recommendation                                                                                                                                                                                                  | Why it matters                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| [Install dbt Core with an adapter](https://docs.getdbt.com/docs/core/pip-install.md#installing-the-adapter) and keep versions in sync | Install with: `python -m pip install dbt-core dbt-ADAPTER_NAME`<br /><br />(For example, `python -m pip install dbt-core dbt-snowflake`)<br /><br />Match adapter versions to your dbt Core version<br /><br /> | Provides a complete, compatible, and ready-to-run dbt setup<br /><br /><br /><br />Prevents runtime errors and adapter incompatibilities |
| For tooling without a warehouse connection, install dbt Core without an adapter                                                       | `python -m pip install dbt-core`                                                                                                                                                                                | Keeps your setup lean, predictable, and easier to maintain                                                                               |
| Use [virtual environments](https://docs.getdbt.com/faqs/Core/install-pip-best-practices.md#using-virtual-environments)                | Install dbt in an isolated environment (for example, `venv`, `pipenv`, `poetry`)                                                                                                                                | Avoids dependency conflicts                                                                                                              |
| Reactivate your virtual environment for each session                                                                                  | Reactivate your virtual environment at the start of each new session before installing dependencies or running dbt commands                                                                                     | Keeps your dbt setup predictable, isolated, and reproducible                                                                             |
| [Create a project](https://docs.getdbt.com/docs/core/installation-overview.md#create-a-project)                                       | Use the `dbt init` command to create and initialize your first project                                                                                                                                          | Creates a standard dbt project and verifies your installation                                                                            |
| Ensure you have the latest versions of `pip`, `wheel`, and `setuptools`                                                               | Before installing dbt, upgrade your Python packaging tools:<br /><br />`python -m pip install --upgrade pip wheel setuptools`                                                                                   | Helps ensure a smoother, more predictable dbt installation                                                                               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

Note, dbt adapters and dbt Core are versioned and installed independently to prevent unintended changes to an existing dbt Core installation.

### Using virtual environments[​](#using-virtual-environments "Direct link to Using virtual environments")

We recommend using [virtual environments](https://docs.python-guide.org/dev/virtualenvs/) to namespace `pip` modules. Here's an example setup:

```shell

python3 -m venv dbt-env				# create the environment
source dbt-env/bin/activate			# activate the environment for Mac and Linux
dbt-env\Scripts\activate			# activate the environment for Windows
```

If you install `dbt` in a virtual environment, you need to reactivate that same virtual environment each time you create a shell window or session.

*Tip:* You can create an alias for the `source` command in your `$HOME/.bashrc`, `$HOME/.zshrc`, or whichever rc file your shell draws from. For example, you can add a command like `alias env_dbt='source <PATH_TO_VIRTUAL_ENV_CONFIG>/bin/activate'`, replacing `<PATH_TO_VIRTUAL_ENV_CONFIG>` with the path to your virtual environment configuration.

### Using the latest versions[​](#using-the-latest-versions "Direct link to Using the latest versions")

dbt installations are tested using the latest versions of `pip` and `setuptools`. Newer versions have improved behavior around dependency resolution, as well as much faster install times by using precompiled "wheels" when available for your operating system.

Before installing dbt, make sure you have the latest versions:

```shell

python -m pip install --upgrade pip wheel setuptools
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
