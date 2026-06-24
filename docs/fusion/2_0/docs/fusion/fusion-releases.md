# Fusion releases [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Fusion availability

This page shows release information for local builds of Fusion only. Fusion releases on the dbt platform adhere to the [release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) categories, giving you control over release cadence and stability.

Track current versions and full release history for the dbt Fusion engine. This data updates live from dbt release channels.

Each of the versions on this page links to the matching section in the [dbt Fusion changelog](https://github.com/dbt-labs/dbt-fusion/blob/main/CHANGELOG.md) on GitHub.

## Release channels[​](#release-channels "Direct link to Release channels")

The dbt Fusion engine is distributed through three release channels:

| Channel  | Description                                  | Stability                                                           |
| -------- | -------------------------------------------- | ------------------------------------------------------------------- |
| `latest` | The known `good` stable version              | ✅ Recommended for production                                       |
| `canary` | The latest version to be officially released | ⚠️ Most recent stable version but still undergoing thorough testing |
| `dev`    | The latest development build                 | ❌ May be unstable; may not have passed all internal tests          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## dbt platform Fusion release tracks[​](#dbt-platform-fusion-release-tracks "Direct link to dbt platform Fusion release tracks")

On dbt platform, each [environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md) uses the account default or your chosen **Fusion release track**. Release tracks control how often that environment receives new Fusion builds. They're separate from the local CLI release channels in the previous section.

For cadence, plan availability, and API values (`fusion-nightly`, `fusion-stable`, and more), refer to [Fusion release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md#fusion-release-tracks). To change the release track for an environment, follow [Upgrade dbt in dbt platform](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md).

Live data below is for local CLI channels

The **Current versions** cards and full release list below pull the public Fusion manifest used for *local* installs (`dev`, `canary`, `latest`). You should use [release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) for dbt platform planning.

Updating local Fusion

The following commands apply only to *local* installations of Fusion. They don't affect which Fusion build your dbt platform environments use. Instead, you can set a [Fusion release track](https://github.com/docs/dbt-versions/dbt-release-tracks#fusion-release-tracks) per environment in dbt platform.

Running the system update command without a version flag installs the `latest` stable release:

```shell
dbt system update
```

To install a specific channel or version, pass the `--version` flag:

```shell
dbt system update --version canary    # Install the canary release
dbt system update --version dev       # Install the dev release
dbt system update --version 2.0.0-preview.126     # Install a specific version
```

Release data is not available. Please try rebuilding the site.
