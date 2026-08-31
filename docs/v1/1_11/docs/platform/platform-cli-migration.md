# dbt platform CLI migration

The dbt Fusion engine is taking over the `pip install dbt` namespace. If you rely on the dbt platform CLI, you need to take action to keep using it without interruption.

## What's changing

Starting September 14, 2026 `pip install dbt` will install the dbt Fusion engine, dbt's next-generation Rust-based engine, instead of the dbt platform CLI. This is a one-time namespace change on PyPI, not a dbt platform CLI upgrade.

If you install the dbt platform CLI using pip today and don't pin a version, your next `pip install dbt` or `pip install --upgrade dbt` installs Fusion instead. Fusion is a different engine: it runs locally against a warehouse connection you configure yourself, instead of running your commands against your dbt platform development environment. Commands like `dbt cancel`, `dbt reattach`, `dbt environment`, `dbt sl export`, and `dbt sqlfluff` don't exist in Fusion.

## What to do

If you use the dbt platform CLI and want to keep using it, switch to Homebrew:

```bash
brew untap dbt-labs/dbt
brew tap dbt-labs/dbt-cli
brew install dbt
```

If you have multiple Homebrew taps: `brew install dbt-labs/dbt-cli/dbt`.

If you can't switch to Homebrew right now (for example, a Docker image that only has pip available), pin your version instead:

```bash
pip install dbt==1.0.0.40.18
```

This keeps working indefinitely, but won't receive further dbt platform CLI updates through pip. Homebrew is the supported long-term path.

## General areas of impact

* **Dockerfiles / CI pipelines**: any unpinned `pip install dbt` step. Search your repos for `pip install dbt` without a `==` version pin.
* **Airflow**: `BashOperator`/`KubernetesPodOperator` tasks that install the CLI into the worker image at build time.
* **`requirements.txt` / `Pipfile` / `pyproject.toml`**: any unpinned `dbt` entry.
* **dbt Power User (VS Code extension)**: it shells out to the dbt platform CLI internally. Pin your version the same way; the extension itself doesn't need any separate action.

## FAQ

* **Why is dbt Labs doing this?** Fusion is the new default `dbt` engine going forward. Freeing up the `dbt` name on PyPI for it means new users get Fusion by default, matching how `dbt` behaves everywhere else (Homebrew, standalone install).

* **Will my pinned dbt platform CLI version stop working?** No. Pinned installs keep working. They just won't get new dbt platform CLI releases through pip after September 14. For that, move to Homebrew.

* **How do I tell which one I have installed?** `dbt --version`. The dbt platform CLI prints a version like `0.40.18`. Fusion's version string needs reconfirming post-update. At the time this article was published, the binary still internally identifies as `dbt-fusion X.Y.Z`, which may change.
