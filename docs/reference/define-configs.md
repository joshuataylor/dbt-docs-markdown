# Define configs

Learn how to define configurations for your resources in a dbt project

Depending on the resource type, you can define configurations in a dbt project and also in an installed package by:

<!-- -->

## Config inheritance[​](#config-inheritance "Direct link to Config inheritance")

The most specific config always takes precedence. This generally follows the order above: an in-file `config()` block --> properties defined in a `.yml` file --> config defined in the project file.

Note - Generic data tests work a little differently when it comes to specificity. See [test configs](https://docs.getdbt.com/reference/data-test-configs.md).

Within the project file, configurations are also applied hierarchically. The most specific config always takes precedence. In the project file, for example, configurations applied to a `marketing` subdirectory will take precedence over configurations applied to the entire `jaffle_shop` project. To apply a configuration to a model or directory of models, define the [resource path](https://docs.getdbt.com/reference/resource-configs/resource-path.md) as nested dictionary keys.

Configurations in your root dbt project have *higher* precedence than configurations in installed packages. This enables you to override the configurations of installed packages, providing more control over your dbt runs.

## Combining configs[​](#combining-configs "Direct link to Combining configs")

Most configurations are "clobbered" when applied hierarchically. Whenever a more specific value is available, it will completely replace the less specific value. Note that a few configs have different merge behavior:

* [`tags`](https://docs.getdbt.com/reference/resource-configs/tags.md) are additive. If a model has some tags configured in `dbt_project.yml`, and more tags are applied in its `.sql` file, the final set of tags will include all of them.

* [`meta`](https://docs.getdbt.com/reference/resource-configs/meta.md) dictionaries are merged (a more specific key-value pair replaces a less specific value with the same key).

* When using the [`freshness`](https://docs.getdbt.com/reference/resource-configs/freshness.md) config, a more specific key-value pair replaces a less specific value with the same key.

* [`pre-hook` and `post-hook`](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook.md) are also additive.

* For clobbering and merging configurations that are inherited from multiple levels, the general rules are:

  <!-- -->

  * Node-level configs (more specific) clobber project-level configs (less specific).
  * For sources, table-level configs (more specific) clobber source-level configs (less specific).
  * The root project's configuration in `dbt_project.yml` clobbers configuration within package files. This is so that users can control the behavior of packages they are installing using `dbt deps` without needing to edit the code in those package files directly.

## The `+` prefix[​](#the--prefix "Direct link to the--prefix")

<!-- -->

dbt demarcates between a folder name and a configuration by using a `+` prefix before the configuration name. The `+` prefix is used for configs *only* and applies to `dbt_project.yml` under the corresponding resource key. It doesn't apply to:

* `config()` Jinja macro within a resource file
* config property in a `.yml` file.

For more info, see the [Using the `+` prefix](https://docs.getdbt.com/reference/resource-configs/plus-prefix.md).

<!-- -->

## Example[​](#example "Direct link to Example")

Here's an example that defines both `sources` and `models` for a project:

models/jaffle\_shop.yml

```yml
version: 2

sources:
  - name: raw_jaffle_shop
    description: A replica of the postgres database used to power the jaffle_shop app.
    tables:
      - name: customers
        columns:
          - name: id
            description: Primary key of the table
            data_tests:
              - unique
              - not_null

      - name: orders
        columns:
          - name: id
            description: Primary key of the table
            data_tests:
              - unique
              - not_null

          - name: user_id
            description: Foreign key to customers

          - name: status
            data_tests:
              - accepted_values:
                  arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                    values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']


models:
  - name: stg_jaffle_shop__customers #  Must match the filename of a model -- including case sensitivity.
    config:
      tags: ['pii']
    columns:
      - name: customer_id
        data_tests:
          - unique
          - not_null

  - name: stg_jaffle_shop__orders
    config:
      materialized: view
    columns:
      - name: order_id
        data_tests:
          - unique
          - not_null
      - name: status
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['placed', 'shipped', 'completed', 'return_pending', 'returned']
              config:
                severity: warn
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

You can find an exhaustive list of each supported property and config, broken down by resource type:

* Model [properties](https://docs.getdbt.com/reference/model-properties.md) and [configs](https://docs.getdbt.com/reference/model-configs.md)
* Source [properties](https://docs.getdbt.com/reference/source-properties.md) and [configs](https://docs.getdbt.com/reference/source-configs.md)
* Seed [properties](https://docs.getdbt.com/reference/seed-properties.md) and [configs](https://docs.getdbt.com/reference/seed-configs.md)
* Snapshot [properties](https://docs.getdbt.com/reference/snapshot-properties.md)
* Analysis [properties](https://docs.getdbt.com/reference/analysis-properties.md)
* Macro [properties](https://docs.getdbt.com/reference/macro-properties.md)
* Exposure [properties](https://docs.getdbt.com/reference/exposure-properties.md)

## FAQs[​](#faqs "Direct link to FAQs")

Does my \`.yml\` file containing tests and descriptions need to be named \`schema.yml\`?

No! You can name this file whatever you want (including `whatever_you_want.yml`), so long as:

* The file is in your `models/` directory¹
* The file has `.yml` extension

Check out the [docs](https://docs.getdbt.com/reference/configs-and-properties.md) for more information.

¹If you're declaring properties for seeds, snapshots, or macros, you can also place this file in the related directory — `seeds/`, `snapshots/` and `macros/` respectively.

If I can name these files whatever I'd like, what should I name them?

It's up to you! Here's a few options:

* Default to the existing terminology: `schema.yml` (though this does make it hard to find the right file over time)
* Use the same name as your directory (assuming you're using sensible names for your directories)
* If you test and document one model (or seed, snapshot, macro etc.) per file, you can give it the same name as the model (or seed, snapshot, macro etc.)

Choose what works for your team. We have more recommendations in our guide on [structuring dbt projects](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview.md).

Should I use separate files to declare resource properties, or one large file?

It's up to you:

* Some folks find it useful to have one file per model (or source / snapshot / seed etc)
* Some find it useful to have one per directory, documenting and testing multiple models in one file

Choose what works for your team. We have more recommendations in our guide on [structuring dbt projects](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview.md).

Can I add tests and descriptions in a SQL config block?

dbt has the ability to define node configs in YAML files, in addition to `config()` blocks and `dbt_project.yml`. But the reverse isn't always true: there are some things in `.yml` files that can *only* be defined there.

Certain properties are special, because:

* They have a unique Jinja rendering context
* They create new project resources
* They don't make sense as hierarchical configuration
* They're older properties that haven't yet been redefined as configs

These properties are:

* [`description`](https://docs.getdbt.com/reference/resource-properties/description.md)
* [`tests`](https://docs.getdbt.com/reference/resource-properties/data-tests.md)
* [`docs`](https://docs.getdbt.com/reference/resource-configs/docs.md)
* `columns`
* [`quote`](https://docs.getdbt.com/reference/resource-properties/columns.md#quote)
* [`source` properties](https://docs.getdbt.com/reference/source-properties.md) (e.g. `loaded_at_field`, `freshness`)
* [`exposure` properties](https://docs.getdbt.com/reference/exposure-properties.md) (e.g. `type`, `maturity`)
* [`macro` properties](https://docs.getdbt.com/reference/resource-properties/arguments.md) (e.g. `arguments`)

Why do model and source YAML files always start with \`version: 2\`?

Once upon a time, the structure of these `.yml` files was very different (s/o to anyone who was using dbt back then!). Adding `version: 2` allowed us to make this structure more extensible.

From [dbt Core v1.5](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.5.md#quick-hits>), the top-level `version:` key is optional in all resource YAML files. If present, only `version: 2` is supported.

Also starting in v1.5, both the [`config-version: 2`](https://docs.getdbt.com/reference/project-configs/config-version.md) and the top-level `version:` key in the `dbt_project.yml` are optional.

Resource YAML files do not currently require this config. We only support `version: 2` if it's specified. Although we do not expect to update YAML files to `version: 3` soon, having this config will make it easier for us to introduce new structures in the future

Can I use a YAML file extension?

No. At present, dbt will only search for files with a `.yml` file extension. In a future release of dbt, dbt will also search for files with a `.yaml` file extension.

## Troubleshooting common errors[​](#troubleshooting-common-errors "Direct link to Troubleshooting common errors")

 Invalid test config given in \[model name]

This error occurs when your `.yml` file does not conform to the structure expected by dbt. A full error message might look like:

```text
* Invalid test config given in models/schema.yml near {'namee': 'event', ...}
  Invalid arguments passed to "UnparsedNodeUpdate" instance: 'name' is a required property, Additional properties are not allowed ('namee' was unexpected)
```

While verbose, an error like this should help you track down the issue. Here, the `name` field was provided as `namee` by accident. To fix this error, ensure that your `.yml` conforms to the expected structure described in this guide.

 Invalid syntax in your schema.yml file

If your `.yml` file is not valid yaml, then dbt will show you an error like this:

```text
Runtime Error
  Syntax error near line 6
  ------------------------------
  5  |   - name: events
  6  |     description; "A table containing clickstream events from the marketing website"
  7  |

  Raw Error:
  ------------------------------
  while scanning a simple key
    in "<unicode string>", line 6, column 5:
          description; "A table containing clickstream events from the marketing website"
          ^
```

This error occurred because a semicolon (`;`) was accidentally used instead of a colon (`:`) after the `description` field. To resolve issues like this, find the `.yml` file referenced in the error message and fix any syntax errors present in the file. There are online YAML validators that can be helpful here, but please be mindful of submitting sensitive information to third-party applications!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
