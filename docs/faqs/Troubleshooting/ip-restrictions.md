# I'm receiving a 403 error 'Forbidden: Access denied' when using service tokens

All [service token](https://docs.getdbt.com/docs/dbt-cloud-apis/service-tokens.md) traffic is subject to IP restrictions.

When using a service token, the following 403 response error indicates the IP is not on the allowlist. To resolve this, you should add your third-party integration CIDRs (network addresses) to your allowlist.

The following is an example of the 403 response error:

```json
        {
            "status": {
                "code": 403,
                "is_success": False,
                "user_message": ("Forbidden: Access denied"),
                "developer_message": None,
            },
            "data": {
                "account_id": <account_id>,
                "user_id": <user_id>,
                "is_service_token": <boolean describing if it's a service token request>,
                "account_access_denied": True,
            },
        }
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
