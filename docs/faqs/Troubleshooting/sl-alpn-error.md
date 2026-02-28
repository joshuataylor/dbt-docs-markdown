# I'm receiving an \`Failed ALPN\` error when trying to connect to the dbt Semantic Layer.

If you're receiving a `Failed ALPN` error when trying to connect the dbt Semantic Layer with the various [data integration tools](https://docs.getdbt.com/docs/cloud-integrations/avail-sl-integrations.md) (such as Tableau, DBeaver, Datagrip, ADBC, or JDBC), it typically happens when connecting from a computer behind a corporate VPN or Proxy (like Zscaler or Check Point).

The root cause is typically the proxy interfering with the TLS handshake as the Semantic Layer uses gRPC/HTTP2 for connectivity. To resolve this:

* If your proxy supports gRPC/HTTP2 but isn't configured to allow ALPN, adjust its settings accordingly to allow ALPN. Or create an exception for the dbt domain.
* If your proxy does not support gRPC/HTTP2, add an SSL interception exception for the dbt domain in your proxy settings

This should help in successfully establishing the connection without the Failed ALPN error.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
