# Exit codes

When dbt exits, it will return an exit code of either 0, 1, or 2.

| Exit Code | Condition                                                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0         | The dbt invocation completed without error.                                                                                                                                |
| 1         | The dbt invocation completed with at least one handled error (eg. model syntax error, bad permissions, etc). The run was completed, but some models may have been skipped. |
| 2         | The dbt invocation completed with an unhandled error (eg. ctrl-c, network interruption, etc).                                                                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

While these exit codes may change in the future, a zero exit code will always imply success whereas a nonzero exit code will always imply failure.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
