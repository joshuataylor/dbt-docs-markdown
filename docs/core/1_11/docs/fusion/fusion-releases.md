# Fusion releases [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Fusion availability

This page shows release information for local builds of Fusion only. Fusion releases on the dbt platform adhere to the [release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) categories, giving you control over release cadence and stability.

Track current versions and full release history for the dbt Fusion engine. This data updates live from dbt release channels.

Each of the versions on this page links to the matching section in the [dbt Fusion changelog](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md) on GitHub.

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

### Current versions

#### Dev

[`v2.0.0-preview.193`](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview193 "View this version in the dbt Fusion changelog")

2026-06-23

#### Canary

[`v2.0.0-preview.193`](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview193 "View this version in the dbt Fusion changelog")

2026-06-24

#### Latest

[`v2.0.0-preview.190`](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview190 "View this version in the dbt Fusion changelog")

2026-06-19

### All releases

Search versions...

Status:All (all)

Channel:All (all)

Showing

<!-- -->

152

<!-- -->

of

<!-- -->

152

<!-- -->

releases

[v2.0.0-preview.193](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview193 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **kczimm**Jun 24, 2026, 02:36 PM

release canary promotion

[v2.0.0-preview.192](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview192 "View this release in the dbt Fusion changelog")Known BadDevCanaryLatestnightlystable

Released by: **akbog**Jun 18, 2026, 06:22 PM

Planned promotion

[v2.0.0-preview.191](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview191 "View this release in the dbt Fusion changelog")Known BadDevCanarynightly

Released by: **akbog**Jun 12, 2026, 09:14 PM

Planned Promotion

[v2.0.0-preview.190](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview190 "View this release in the dbt Fusion changelog")GoodDevCanarynightlystableLatestST Mondayst-monday-stableST Wednesdayst-wednesday-stableST Thursdayst-thursday-stable

Released by: **agelber-dbt**Jun 22, 2026, 09:07 PM

Automated promotion

[v2.0.0-preview.189](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview189 "View this release in the dbt Fusion changelog")GoodDevCanaryLateststable

Released by: **akbog**Jun 11, 2026, 03:15 AM

Planned Promotion

[v2.0.0-preview.188](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview188 "View this release in the dbt Fusion changelog")Known BadDevCanarystablenightlyst-monday-stable

Released by: **fa-assistant**Jun 8, 2026, 07:05 PM

Automated ST snapshot

[v2.0.0-preview.187](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview187 "View this release in the dbt Fusion changelog")Known BadDev

Released by: **akbog**Jun 6, 2026, 02:06 AM

Automated promotion

[v2.0.0-preview.186](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview186 "View this release in the dbt Fusion changelog")GoodDevnightlyCanarystableLatestST Mondayst-monday-stableextendedST Thursdayst-thursday-stablest-monday-extendedst-wednesday-extendedst-thursday-extended

Released by: **agelber-dbt**Jun 22, 2026, 09:07 PM

Automated promotion

[v2.0.0-preview.185](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview185 "View this release in the dbt Fusion changelog")GoodDev

Released by: **akbog**Jun 3, 2026, 07:49 AM

Automated promotion

[v2.0.0-preview.184](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview184 "View this release in the dbt Fusion changelog")GoodDevCanarystableLatestst-thursday-stable

Released by: **fa-assistant**Jun 4, 2026, 09:34 PM

Automated ST snapshot

[v2.0.0-preview.183](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview183 "View this release in the dbt Fusion changelog")GoodDevCanaryLateststablenightlyst-wednesday-stable

Released by: **fa-assistant**Jun 3, 2026, 10:03 PM

Automated ST snapshot

[v2.0.0-preview.182](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview182 "View this release in the dbt Fusion changelog")GoodDevCanaryLateststablenightlyST Mondayst-monday-stableST WednesdayST Thursday

Released by: **fa-assistant**Jun 4, 2026, 09:32 PM

Automated ST promotion

[v2.0.0-preview.181](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview181 "View this release in the dbt Fusion changelog")GoodDevCanarystableLatestnightly

Released by: **akbog**Jun 1, 2026, 06:55 AM

Planned Promotion

[v2.0.0-preview.180](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview180 "View this release in the dbt Fusion changelog")GoodDevCanarystableLatestnightly

Released by: **akbog**May 30, 2026, 06:35 AM

Planned promotion

[v2.0.0-preview.179](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview179 "View this release in the dbt Fusion changelog")GoodDev

Released by: **kczimm**May 29, 2026, 05:01 PM

Automated promotion

[v2.0.0-preview.178](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview178 "View this release in the dbt Fusion changelog")GoodDevCanaryLateststablenightlyst-wednesday-stablest-thursday-stable

Released by: **fa-assistant**May 29, 2026, 01:12 PM

Automated ST snapshot

[v2.0.0-preview.177](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview177 "View this release in the dbt Fusion changelog")GoodDevCanarystableLatestnightlyst-thursday-stableST Mondayst-monday-stableST WednesdayST Thursday

Released by: **fa-assistant**May 29, 2026, 01:10 PM

Automated ST promotion

[v2.0.0-preview.176](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview176 "View this release in the dbt Fusion changelog")GoodDevCanarynightlystableLatestst-wednesday-stablest-thursday-stableST MondayST WednesdayST Thursday

Released by: **fa-assistant**May 21, 2026, 09:31 PM

Automated ST promotion

[v2.0.0-preview.175](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview175 "View this release in the dbt Fusion changelog")GoodDevCanaryLateststablest-thursday-stablest-monday-stablest-wednesday-stableST MondayextendedfallbacknightlyST WednesdayST Thursdayst-monday-extendedst-monday-fallbackst-wednesday-extendedst-wednesday-fallbackst-thursday-extendedst-thursday-fallback

Released by: **agelber-dbt**Jun 22, 2026, 09:07 PM

Automated promotion

[v2.0.0-preview.174](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview174 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Apr 29, 2026, 03:25 PM

Planned orchestration promotion

[v2.0.0-preview.173](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview173 "View this release in the dbt Fusion changelog")release candidateGoodDevCanaryLatestST MondaynightlystableST WednesdayST Thursday

Released by: **TimKlense**Apr 30, 2026, 11:02 PM

manually updated for thursday

[v2.0.0-preview.172](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview172 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Apr 15, 2026, 04:28 PM

Planned orchestration promotion

[v2.0.0-preview.171](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview171 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Apr 16, 2026, 09:21 PM

Automated ST promotion

[v2.0.0-preview.170](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview170 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestextendedfallbackst-thursday-extendedst-thursday-fallbackst-monday-extendedst-monday-fallbackst-wednesday-extendedst-wednesday-fallback

Released by: **agelber-dbt**May 1, 2026, 02:22 PM

Preparing for GA

[v2.0.0-preview.169](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview169 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Apr 8, 2026, 12:06 PM

Automated promotion

[v2.0.0-preview.168](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview168 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **peter-bertuglia**Apr 8, 2026, 08:04 PM

Planned orchestration promotion

[v2.0.0-preview.167](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview167 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Apr 7, 2026, 12:17 PM

Automated promotion

[v2.0.0-preview.166](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview166 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **peter-bertuglia**Apr 7, 2026, 04:34 PM

Planned orchestration promotion

[v2.0.0-preview.165](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview165 "View this release in the dbt Fusion changelog")GoodDev

Released by: **akbog**Apr 4, 2026, 03:16 AM

Automated promotion

[v2.0.0-preview.164](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview164 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Apr 9, 2026, 09:21 PM

Automated ST promotion

[v2.0.0-preview.163](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview163 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Mar 26, 2026, 03:46 PM

Planned orchestration promotion

[v2.0.0-preview.162](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview162 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **dataders**Mar 23, 2026, 04:19 PM

Automated promotion

[v2.0.0-preview.161](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview161 "View this release in the dbt Fusion changelog")Known BadDevCanaryLatest

Released by: **james-durand-dbt**Mar 24, 2026, 04:22 PM

Planned orchestration promotion

[v2.0.0-preview.160](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview160 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **dataders**Mar 22, 2026, 07:14 PM

Automated promotion

[v2.0.0-preview.159](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview159 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **github-merge-queue**Mar 20, 2026, 12:15 PM

Automated promotion

[v2.0.0-preview.158](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview158 "View this release in the dbt Fusion changelog")Known BadDevCanaryLatest

Released by: **kylepeirce**Mar 20, 2026, 03:50 PM

Planned orchestration promotion

[v2.0.0-preview.157](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview157 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **jasonlin45**Mar 18, 2026, 08:35 PM

Automated promotion

[v2.0.0-preview.156](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview156 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **kczimm**Mar 17, 2026, 08:55 PM

Automated promotion

[v2.0.0-preview.155](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview155 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue**Mar 16, 2026, 12:20 PM

Automated promotion

[v2.0.0-preview.154](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview154 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Mar 26, 2026, 09:14 PM

Automated ST promotion

[v2.0.0-preview.153](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview153 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **kylepeirce**Mar 12, 2026, 10:37 PM

Bug fix for INC-5687

[v2.0.0-preview.152](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview152 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue**Mar 12, 2026, 12:13 PM

Automated promotion

[v2.0.0-preview.151](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview151 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **kylepeirce**Mar 12, 2026, 06:52 PM

Planned orchestration promotion

[v2.0.0-preview.150](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview150 "View this release in the dbt Fusion changelog")GoodDev

Released by: **kczimm**Mar 10, 2026, 10:53 PM

Automated promotion

[v2.0.0-preview.149](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview149 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Mar 10, 2026, 03:29 PM

Planned orchestration promotion

[v2.0.0-preview.148](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview148 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Mar 12, 2026, 10:10 PM

Automated ST promotion

[v2.0.0-preview.147](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview147 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **kczimm**Mar 7, 2026, 02:04 AM

Automated promotion

[v2.0.0-preview.146](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview146 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **kczimm**Mar 5, 2026, 11:58 PM

Automated promotion

[v2.0.0-preview.145](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview145 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **venkaa28**Mar 6, 2026, 04:22 AM

INC-5500

[v2.0.0-preview.144](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview144 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **peter-bertuglia**Mar 4, 2026, 08:41 PM

retrying scheduled promo for vscode ext

[v2.0.0-preview.143](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview143 "View this release in the dbt Fusion changelog")Known BadDevCanaryLatestST Monday

Released by: **akbog**Mar 3, 2026, 03:43 PM

inc-5454-minor-table-not-found-in-schema-errors

[v2.0.0-preview.142](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview142 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue**Feb 27, 2026, 12:02 PM

Automated promotion

[v2.0.0-preview.141](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview141 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **jasonlin45**Feb 26, 2026, 10:31 PM

Automated promotion

[v2.0.0-preview.139](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview139 "View this release in the dbt Fusion changelog")GoodDev

Released by: **jasonlin45**Feb 26, 2026, 06:47 PM

Automated promotion

[v2.0.0-preview.137](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview137 "View this release in the dbt Fusion changelog")GoodDev

Released by: **jasonlin45**Feb 26, 2026, 01:55 AM

Automated promotion

[v2.0.0-preview.135](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview135 "View this release in the dbt Fusion changelog")GoodDev

Released by: **chayac**Feb 25, 2026, 12:45 AM

Automated promotion

[v2.0.0-preview.134](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview134 "View this release in the dbt Fusion changelog")GoodDev

Released by: **chayac**Feb 24, 2026, 10:42 PM

Automated promotion

[v2.0.0-preview.127](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview127 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Feb 20, 2026, 12:17 PM

Automated promotion

[v2.0.0-preview.126](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview126 "View this release in the dbt Fusion changelog")Known BadDevCanaryLatest

Released by: **mikaylacrawford**Feb 20, 2026, 07:38 PM

Planned orchestration promotion

[v2.0.0-preview.125](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview125 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue\[bot]**&#x46;eb 19, 2026, 06:05 PM

Automated promotion

[v2.0.0-preview.123](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview123 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue**Feb 18, 2026, 12:06 PM

Automated promotion

[v2.0.0-preview.121](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview121 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue**Feb 16, 2026, 12:09 PM

Automated promotion

[v2.0.0-preview.120](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview120 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Mar 5, 2026, 10:13 PM

Automated ST promotion

[v2.0.0-preview.119](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview119 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **dataders**Feb 13, 2026, 06:48 PM

Automated promotion

[v2.0.0-preview.118](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview118 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Feb 13, 2026, 12:15 PM

Automated promotion

[v2.0.0-preview.117](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview117 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **jzhu13**Feb 13, 2026, 12:45 AM

Automated promotion

[v2.0.0-preview.116](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview116 "View this release in the dbt Fusion changelog")GoodDev

Released by: **jzhu13**Feb 12, 2026, 07:36 PM

Automated promotion

[v2.0.0-preview.114](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview114 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Feb 26, 2026, 10:15 PM

Automated ST promotion

[v2.0.0-preview.110](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview110 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **peter-bertuglia**Feb 6, 2026, 07:34 PM

INC-5104

[v2.0.0-preview.108](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview108 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **peter-bertuglia**Feb 5, 2026, 04:42 PM

Planned orchestration promotion

[v2.0.0-preview.105](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview105 "View this release in the dbt Fusion changelog")GoodDev

Released by: **github-merge-queue**Jan 30, 2026, 01:12 PM

Automated promotion

[v2.0.0-preview.104](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview104 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Feb 12, 2026, 10:13 PM

Automated ST promotion

[v2.0.0-preview.103](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview103 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Jan 27, 2026, 01:09 PM

Automated promotion

[v2.0.0-preview.102](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview102 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Jan 27, 2026, 08:06 PM

Planned orchestration promotion

[v2.0.0-preview.101](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview101 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Jan 29, 2026, 10:12 PM

Automated ST promotion

[v2.0.0-preview.100](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview100 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Jan 21, 2026, 01:42 PM

Automated promotion

[v2.0.0-preview.99](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview99 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **akbog**Jan 21, 2026, 07:10 AM

Automated promotion

[v2.0.0-preview.98](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview98 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **github-merge-queue**Jan 19, 2026, 01:22 PM

Automated promotion

[v2.0.0-preview.97](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview97 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Jan 16, 2026, 01:14 PM

Automated promotion

[2.0.0-preview.97](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview97 "View this release in the dbt Fusion changelog")Known Bad

[v2.0.0-preview.96](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview96 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Jan 22, 2026, 10:10 PM

Automated ST promotion

[v2.0.0-preview.95](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview95 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **jasonlin45**Jan 15, 2026, 06:16 AM

Automated promotion

[v2.0.0-preview.94](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview94 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Jan 13, 2026, 07:26 PM

Planned orchestration promotion

[v2.0.0-preview.93](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview93 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **github-merge-queue**Jan 9, 2026, 01:24 PM

Automated promotion

[v2.0.0-preview.92](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview92 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Jan 15, 2026, 10:10 PM

Automated ST promotion

[v2.0.0-preview.91](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview91 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Dec 22, 2025, 01:09 PM

Automated promotion

[v2.0.0-preview.90](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview90 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Dec 19, 2025, 01:04 PM

Automated promotion

[v2.0.0-preview.89](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview89 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **laconc**Dec 24, 2025, 12:06 AM

st release

[v2.0.0-preview.88](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview88 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Dec 18, 2025, 04:20 PM

Planned orchestration promotion

[v2.0.0-preview.87](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview87 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue\[bot]**&#x44;ec 16, 2025, 01:54 PM

Automated promotion

[v2.0.0-preview.86](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview86 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **james-durand-dbt**Dec 15, 2025, 08:35 PM

INC-4737

[v2.0.0-preview.85](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview85 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Dec 18, 2025, 10:10 PM

Automated ST promotion

[v2.0.0-preview.84](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview84 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **james-durand-dbt**Dec 12, 2025, 06:04 PM

Planned orchestration promotion

[v2.0.0-preview.83](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview83 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Dec 10, 2025, 04:19 PM

Planned orchestration promotion

[v2.0.0-preview.82](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview82 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **xuliangs**Dec 9, 2025, 07:41 PM

Automated promotion

[v2.0.0-preview.81](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview81 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **ddk-dbt**Dec 9, 2025, 03:21 PM

Automated promotion

[v2.0.0-preview.80](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview80 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **venkaa28**Dec 8, 2025, 07:34 PM

Automated promotion

[v2.0.0-preview.79](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview79 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **ddk-dbt**Dec 5, 2025, 03:18 PM

Automated promotion

[v2.0.0-preview.78](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview78 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **ddk-dbt**Dec 4, 2025, 03:24 PM

Automated promotion

[v2.0.0-preview.77](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview77 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **ddk-dbt**Dec 3, 2025, 10:26 PM

Automated promotion

[v2.0.0-preview.76](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview76 "View this release in the dbt Fusion changelog")Known BadDevCanaryLatest

Released by: **venkaa28**Dec 4, 2025, 01:36 AM

preview\.77 causing issues for IA. rolling back for incident.

[v2.0.0-preview.75](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview75 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **jasonlin45**Nov 22, 2025, 04:09 AM

Automated promotion

[v2.0.0-preview.74](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview74 "View this release in the dbt Fusion changelog")GoodDev

Released by: **ddk-dbt**Nov 21, 2025, 03:05 PM

Automated promotion

[v2.0.0-preview.73](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview73 "View this release in the dbt Fusion changelog")GoodDev

Released by: **ddk-dbt**Nov 20, 2025, 03:19 PM

Automated promotion

[v2.0.0-preview.72](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview72 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Dec 11, 2025, 10:11 PM

Automated ST promotion

[v2.0.0-preview.71](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview71 "View this release in the dbt Fusion changelog")GoodDev

Released by: **venkaa28**Nov 19, 2025, 02:06 AM

Automated promotion

[v2.0.0-preview.70](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview70 "View this release in the dbt Fusion changelog")GoodDev

Released by: **ddk-dbt**Nov 17, 2025, 03:03 PM

Automated promotion

[v2.0.0-preview.69](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview69 "View this release in the dbt Fusion changelog")GoodDev

Released by: **ChenyuLInx**Nov 15, 2025, 06:57 AM

Automated promotion

[v2.0.0-preview.68](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview68 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **dbtlabs007**Nov 18, 2025, 12:44 AM

Planned orchestration promotion

[v2.0.0-preview.67](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview67 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **laconc**Nov 20, 2025, 10:47 PM

Thursday ST release, manual run

[v2.0.0-preview.66](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview66 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **mishamsk**Nov 12, 2025, 02:09 PM

Automated promotion

[v2.0.0-preview.65](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview65 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Nov 12, 2025, 10:00 PM

Planned orchestration promotion

[v2.0.0-preview.63](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview63 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **ddk-dbt**Nov 13, 2025, 11:05 PM

scheduled promotion

[v2.0.0-preview.62](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview62 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **jzhu13**Nov 5, 2025, 11:18 PM

Automated promotion

[v2.0.0-preview.61](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview61 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **ddk-dbt**Nov 5, 2025, 02:44 PM

Automated promotion

[v2.0.0-preview.60](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview60 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Nov 5, 2025, 08:15 PM

Planned orchestration promotion

[v2.0.0-preview.59](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview59 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Nov 5, 2025, 02:50 PM

Planned orchestration promotion

[v2.0.0-preview.58](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview58 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **ddk-dbt**Nov 3, 2025, 02:39 PM

Automated promotion

[v2.0.0-preview.57](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview57 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **fa-assistant**Nov 6, 2025, 10:09 PM

Automated ST promotion

[v2.0.0-preview.56](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview56 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Oct 31, 2025, 03:24 PM

Planned orchestration promotion

[v2.0.0-preview.55](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview55 "View this release in the dbt Fusion changelog")GoodDev

Released by: **dataders**Oct 30, 2025, 07:08 PM

Automated promotion

[v2.0.0-preview.54](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview54 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **ddk-dbt**Oct 30, 2025, 03:24 PM

Automated promotion

[v2.0.0-preview.53](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview53 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Oct 30, 2025, 04:47 PM

Planned orchestration promotion

[v2.0.0-preview.52](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview52 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **ddk-dbt**Oct 29, 2025, 02:37 PM

Automated promotion

[v2.0.0-preview.51](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview51 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **dataders**Oct 29, 2025, 01:13 AM

Automated promotion

[v2.0.0-preview.50](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview50 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **peter-bertuglia**Oct 28, 2025, 06:46 PM

Planned orchestration promotion

[v2.0.0-preview.49](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview49 "View this release in the dbt Fusion changelog")GoodDev

Released by: **jzhu13**Oct 22, 2025, 06:12 PM

Automated promotion

[v2.0.0-preview.48](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview48 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST Monday

Released by: **fa-assistant**Oct 27, 2025, 06:13 PM

Automated promotion

[v2.0.0-preview.47](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview47 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Oct 22, 2025, 09:27 PM

Planned orchestration promotion

[v2.0.0-preview.45](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview45 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST Wednesday

Released by: **johnchappelledbt**Oct 22, 2025, 10:16 PM

10-22-25 Wed ST release

[v2.0.0-preview.44](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview44 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **dbtlabs007**Oct 9, 2025, 07:39 PM

inc-3966-informational-releasing-dbt-lsp-hotfix-to-fix-hang-coalesce-thaw

[v2.0.0-preview.43](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview43 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Oct 9, 2025, 03:56 PM

fs deployment freeze promotion

[v2.0.0-preview.42](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview42 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Oct 8, 2025, 02:26 PM

Automated promotion

[v2.0.0-preview.41](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview41 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **ddk-dbt**Oct 8, 2025, 09:45 PM

pre-coalesce sync

[v2.0.0-preview.40](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview40 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **davidharting**Oct 7, 2025, 09:56 PM

Automated promotion

[v2.0.0-preview.39](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview39 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue**Oct 7, 2025, 02:33 PM

Automated promotion

[v2.0.0-preview.38](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview38 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Oct 7, 2025, 04:19 PM

orc release

[v2.0.0-preview.37](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview37 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue\[bot]**&#x4F;ct 6, 2025, 02:40 PM

Automated promotion

[v2.0.0-preview.36](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview36 "View this release in the dbt Fusion changelog")Known BadDevCanary

Released by: **github-merge-queue**Oct 3, 2025, 02:21 PM

Automated promotion

[v2.0.0-preview.35](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview35 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST Monday

Released by: **fa-assistant**Oct 6, 2025, 06:13 PM

Automated promotion

[v2.0.0-preview.34](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview34 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Oct 3, 2025, 03:58 PM

Planned orchestration promotion

[v2.0.0-preview.33](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview33 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **ayshukla**Oct 2, 2025, 06:23 PM

Planned orchestration promotion

[v2.0.0-preview.32](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview32 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Oct 1, 2025, 03:21 PM

Planned orchestration promotion

[v2.0.0-preview.31](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview31 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **github-merge-queue\[bot]**&#x53;ep 30, 2025, 02:44 PM

Automated promotion

[v2.0.0-preview.30](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview30 "View this release in the dbt Fusion changelog")GoodDevCanaryLatest

Released by: **mikaylacrawford**Sep 30, 2025, 04:01 PM

Planned orchestration promotion

[v2.0.0-preview.29](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview29 "View this release in the dbt Fusion changelog")GoodDevCanaryLatestST MondayST WednesdayST Thursday

Released by: **ddk-dbt**Oct 3, 2025, 03:26 PM

Automated promotion

[v2.0.0-preview.28](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview28 "View this release in the dbt Fusion changelog")GoodDev

Sep 25, 2025, 02:08 PM

[v2.0.0-preview.27](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview27 "View this release in the dbt Fusion changelog")Known Bad

[v2.0.0-preview.26](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview26 "View this release in the dbt Fusion changelog")GoodCanary

Sep 25, 2025, 03:16 PM

[v2.0.0-preview.25](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview25 "View this release in the dbt Fusion changelog")GoodST MondayST WednesdayLatestST Thursday

Sep 25, 2025, 10:03 PM

[v2.0.0-preview.23](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview23 "View this release in the dbt Fusion changelog")Known Bad

[v2.0.0-preview-nightly.176](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md#200-preview-nightly176 "View this release in the dbt Fusion changelog")GoodDevCanary

Released by: **akbog**May 12, 2026, 08:21 PM

Rollback incorrect release of 2.0.0
