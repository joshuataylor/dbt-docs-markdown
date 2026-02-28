# I'm receiving a NoneType object has no attribute error in the IDE?

If you're unable to access the Studio IDE due to the below error message, we'll do our best to get you unstuck with the below steps!

```shell
NoneType object has no attribute 
enumerate_fields'
```

Usually this error indicates that you tried connecting your database via [SSH tunnel](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-redshift.md#connecting-using-an-ssh-tunnel). If you're seeing this error, double-check you have supplied the following items:

* the hostname
* username
* port of bastion server

If you've tried the step above and are still experiencing this behavior - reach out to the Support team at <support@getdbt.com> and we'll be happy to help!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
