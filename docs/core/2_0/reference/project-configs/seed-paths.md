# seed-paths

dbt\_project.yml

```yml
seed-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [seed](https://docs.getdbt.com/docs/build/seeds.md) files are located.

## Default[​](#default "Direct link to Default")

By default, dbt expects seeds to be located in the `seeds` directory. For example, `seed-paths: ["seeds"]`.

<!-- -->

Paths specified in `seed-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/seed`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    seed-paths: ["seed"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    seed-paths: ["/Users/username/project/seed"]
    ```

## Examples[​](#examples "Direct link to Examples")

### Use a directory named `custom_seeds` instead of `seeds`[​](#use-a-directory-named-custom_seeds-instead-of-seeds "Direct link to use-a-directory-named-custom_seeds-instead-of-seeds")

dbt\_project.yml

```yml
seed-paths: ["custom_seeds"]
```

### Co-locate your models and seeds in the `models` directory[​](#co-locate-your-models-and-seeds-in-the-models-directory "Direct link to co-locate-your-models-and-seeds-in-the-models-directory")

Note: this works because dbt is looking for different file types for seeds (`.csv` files) and models (`.sql` files).

dbt\_project.yml

```yml
seed-paths: ["models"]
model-paths: ["models"]
```

### Split your seeds across two directories[​](#split-your-seeds-across-two-directories "Direct link to Split your seeds across two directories")

Note: We recommend that you instead use two subdirectories within the `seeds/` directory to achieve a similar effect.

dbt\_project.yml

```yml
seed-paths: ["seeds", "custom_seeds"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
