# Infer configurations

## Authentication[​](#authentication "Direct link to Authentication")

To connect to Infer from your dbt instance you need to set up a correct profile in your `profiles.yml`.

The format of this should look like this:

\~/.dbt/profiles.yml

```yaml
<profile-name>:
  target: <target-name>
  outputs:
    <target-name>:
      type: infer
      url: "<infer-api-endpoint>"
      username: "<infer-api-username>"
      apikey: "<infer-apikey>"
      data_config:
        [configuration for your underlying data warehouse]  
```

### Description of Infer Profile Fields[​](#description-of-infer-profile-fields "Direct link to Description of Infer Profile Fields")

| Field         | Required | Description                                                                                                                                       |
| ------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`        | Yes      | Must be set to `infer`. This must be included either in `profiles.yml` or in the `dbt_project.yml` file.                                          |
| `url`         | Yes      | The host name of the Infer server to connect to. Typically this is `https://app.getinfer.io`.                                                     |
| `username`    | Yes      | Your Infer username - the one you use to login.                                                                                                   |
| `apikey`      | Yes      | Your Infer api key.                                                                                                                               |
| `data_config` | Yes      | The configuration for your underlying data warehouse. The format of this follows the format of the configuration for your data warehouse adapter. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
