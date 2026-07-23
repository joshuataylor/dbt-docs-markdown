# Clone the Jaffle Shop sample project

[Back to guides](https://docs.getdbt.com/guides.md)

dbt

Beginner

Beginner

[Menu ]()



## What is Jaffle Shop?[​](#what-is-jaffle-shop "Direct link to What is Jaffle Shop?")

[Jaffle Shop](https://github.com/dbt-labs/jaffle-shop) is dbt Labs' canonical sample dbt project for a fictional cafe business. This guide shows you how to clone the project using Git from GitHub or GitLab.

Cloning downloads the dbt project files to your machine: models, seeds, tests, and configuration. It doesn't install dbt or set up a database. If you only need the project files, follow the [Clone the repository](#clone-the-repository) and [Verify the clone](#verify-the-clone) sections.

To run dbt commands like `dbt seed`, `dbt run`, and `dbt test`, you also need dbt installed and a database to connect to. Refer to [Next steps](#next-steps) for links to warehouse quickstarts.

### Related content[​](#related-content "Direct link to Related content")

* [Install dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md)
* [About dbt projects](https://docs.getdbt.com/docs/build/projects.md)
* [Example dbt projects](https://docs.getdbt.com/faqs/Project/example-projects.md)

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [Git](https://git-scm.com/downloads) installed
* A terminal
* A [GitHub](https://github.com/join) or [GitLab](https://gitlab.com/users/sign_up) account (only if you plan to fork the repo, push changes, or clone from a private repository)

Verify Git is installed:

```bash
git --version
```

## Clone the repository[​](#clone-the-repository "Direct link to Clone the repository")

* GitHub
* GitLab
* Other platforms

1. Open your terminal and navigate to where you keep projects:

   ```bash
   cd ~/Documents/Github
   ```

2. Clone the repository:

   ```bash
   git clone https://github.com/dbt-labs/jaffle-shop.git
   ```

3. Change into the project directory:

   ```bash
   cd jaffle-shop
   ```

If your organization hosts Jaffle Shop on GitLab, or you've forked the repo there, clone from your GitLab URL instead:

1. Open your terminal and navigate to where you keep projects:

   ```bash
   cd ~/Documents/Github
   ```

2. Clone the repository (replace `YOUR_USERNAME` with your GitLab username or group):

   ```bash
   git clone https://gitlab.com/YOUR_USERNAME/jaffle-shop.git
   ```

3. Change into the project directory:

   ```bash
   cd jaffle-shop
   ```

If your organization has mirrored or forked Jaffle Shop on Bitbucket, Azure DevOps, or another Git host, use the clone URL from that platform. The `git clone` command works the same way:

1. Open your terminal and navigate to where you keep projects:

   ```bash
   cd ~/Documents/Github
   ```

2. Clone the repository:

   ```bash
   git clone <your-repo-clone-url>
   ```

3. Change into the project directory:

   ```bash
   cd jaffle-shop
   ```

## Verify the clone[​](#verify-the-clone "Direct link to Verify the clone")

Confirm the project files are present:

```bash
ls
```

You should see files and folders including:

* `dbt_project.yml`
* `models/`
* `seeds/`
* `packages.yml`

Cloning is complete! 🎉 You now have the Jaffle Shop project files on your machine.

## Explore the project structure[​](#explore-the-project-structure "Direct link to Explore the project structure")

Before you install dbt or connect a database, take a quick look at what you cloned. Jaffle Shop is a fictional cafe business. The project contains 13 models that transform cafe data about customers, locations (stores), products, supplies, and orders.

It also includes:

* **Seeds:** Six CSV files under `seeds/jaffle-data/` that provide the raw sample data.
* **Data tests and unit tests:** YAML alongside the models that check uniqueness, not-null values, and a few unit-test cases.
* **Macros:** Small helper macros in `macros/`.
* **Packages:** Dependencies listed in `packages.yml` (install later with `dbt deps`).

A simplified view of the project looks like this:

```text
jaffle-shop/
├── dbt_project.yml
├── packages.yml
├── models/
│   ├── staging/          # Clean and rename raw tables
│   │   ├── stg_customers.sql
│   │   ├── stg_orders.sql
│   │   ├── stg_order_items.sql
│   │   ├── stg_products.sql
│   │   ├── stg_locations.sql
│   │   ├── stg_supplies.sql
│   │   └── __sources.yml
│   └── marts/            # Business-ready tables for the cafe
│       ├── customers.sql
│       ├── orders.sql
│       ├── order_items.sql
│       ├── products.sql
│       ├── locations.sql
│       ├── supplies.sql
│       └── metricflow_time_spine.sql
├── seeds/
│   └── jaffle-data/      # Sample CSV data (customers, orders, and more)
├── macros/
├── analyses/
└── data-tests/
```

Staging models sit closest to the raw seed data. Marts models join and shape that data into the tables you use for analysis. You don't need to read every file yet. This layout follows a standard [dbt project](https://docs.getdbt.com/docs/build/projects.md) pattern that appears in many real-world projects.

## Next steps[​](#next-steps "Direct link to Next steps")

To run or develop the project, you need dbt installed and a database connected. These links can help:

* **[Install self-hosted dbt](https://docs.getdbt.com/docs/local/install-dbt.md):** Cloning doesn't install dbt. You need it to run commands.
* **[Set up a virtual environment](https://docs.getdbt.com/docs/local/install-dbt.md):** Keeps dbt separate from other Python projects on your machine.
* **[About dbt deps command](https://docs.getdbt.com/reference/commands/deps.md):** The repo lists packages in `packages.yml`. Run `dbt deps` after you install dbt.
* **[About dbt projects](https://docs.getdbt.com/docs/build/projects.md):** Learn the project structure before editing models.
* **[dbt Learn](https://learn.getdbt.com/):** Interactive courses for new users.

### Warehouse quickstarts[​](#warehouse-quickstarts "Direct link to Warehouse quickstarts")

To run the project, you need a database and adapter configured in `profiles.yml`. Choose the quickstart for your warehouse or local setup:

* [Quickstart for dbt and Snowflake](https://docs.getdbt.com/guides/snowflake.md)
* [Quickstart for dbt and BigQuery](https://docs.getdbt.com/guides/bigquery.md)
* [Quickstart for dbt and Databricks](https://docs.getdbt.com/guides/databricks.md)
* [Quickstart for dbt and Redshift](https://docs.getdbt.com/guides/redshift.md)
* [Quickstart for dbt Core using DuckDB](https://docs.getdbt.com/guides/duckdb.md): Clone [`jaffle_shop_duckdb`](https://github.com/dbt-labs/jaffle_shop_duckdb) and follow the guide.

You can also browse [all guides](https://docs.getdbt.com/guides.md) or other [example dbt projects](https://docs.getdbt.com/faqs/Project/example-projects.md).

## Optional cleanup[​](#optional-cleanup "Direct link to Optional cleanup")

If you cloned the repo only to test these steps and don't need the project anymore, you can remove the folder:

```bash
rm -rf ~/Documents/Github/jaffle-shop
```

This deletes the cloned project from your machine. It doesn't affect the GitHub or GitLab repository.
