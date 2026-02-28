# Exposure properties

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [Using exposures](https://docs.getdbt.com/docs/build/exposures.md)
* [Declaring resource properties](https://docs.getdbt.com/reference/configs-and-properties.md)

## Overview[​](#overview "Direct link to Overview")

<!-- -->

Exposures are defined in `properties.yml` files nested under an `exposures:` key. You may define `exposures` in YAML files that also define `sources` or `models`. Exposure properties<!-- --> are "special properties" in that you can't configure them in the `dbt_project.yml` file or using `config()` blocks. Refer to [Configs and properties](https://docs.getdbt.com/reference/define-properties#which-properties-are-not-also-configs) for more info.<br />

Note that while most exposure properties must be configured directly in these YAML files, you can set the [`enabled`](https://docs.getdbt.com/reference/resource-configs/enabled.md) config at the [project level](#project-level-configs) in the`dbt_project.yml` file.

You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within the `models/` directory.

Exposure names must contain only letters, numbers, and underscores (no spaces or special characters). For a short human-friendly name with title casing, spaces, and special characters, use the `label` property.

models/\<filename>.yml

```yml

exposures:
  - name: <string_with_underscores>
    description: <markdown_string>
    type: {dashboard, notebook, analysis, ml, application}
    url: <string>
    maturity: {high, medium, low}  # Indicates level of confidence or stability in the exposure
    enabled: true | false
    config: # 'tags' and 'meta' changed to config in v1.10
      tags: [<string>] 
      meta: {<dictionary>}
      enabled: true | false
    owner: # supports 'name' and 'email' only
      name: <string>
      email: <string>
    
    depends_on:
      - ref('model')
      - ref('seed')
      - source('name', 'table')
      - metric('metric_name')
      
    label: "Human-Friendly Name for this Exposure!"

  - name: ... # declare properties of additional exposures
```

## Example[​](#example "Direct link to Example")

models/jaffle/exposures.yml

```yaml

exposures:

  - name: weekly_jaffle_metrics
    label: Jaffles by the Week              # optional
    type: dashboard                         # required
    maturity: high                          # optional
    url: https://bi.tool/dashboards/1       # optional
    description: >                          # optional
      Did someone say "exponential growth"?

    depends_on:                             # expected
      - ref('fct_orders')
      - ref('dim_customers')
      - source('gsheets', 'goals')
      - metric('count_orders')

    owner:
      name: Callum McData
      email: data@jaffleshop.com


      
  - name: jaffle_recommender
    maturity: medium
    type: ml
    url: https://jupyter.org/mycoolalg
    description: >
      Deep learning to power personalized "Discover Sandwiches Weekly"
    
    depends_on:
      - ref('fct_orders')
      
    owner:
      name: Data Science Drew
      email: data@jaffleshop.com

      
  - name: jaffle_wrapped
    type: application
    description: Tell users about their favorite jaffles of the year
    depends_on: [ ref('fct_orders') ]
    owner: { email: summer-intern@jaffleshop.com }
```

#### Project-level configs[​](#project-level-configs "Direct link to Project-level configs")

You can define project-level configs for exposures in the `dbt_project.yml` file under the `exposures:` key using the `+` prefix. Currently, only the [`enabled` config](https://docs.getdbt.com/reference/resource-configs/enabled.md) is supported:

dbt\_project.yml

```yml
name: 'project_name'

# rest of dbt_project.yml

exposures:
  +enabled: true
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
