# Record timing info

The `-r` or `--record-timing-info` flag saves performance profiling information to a file. This file can be visualized with `snakeviz` to understand the performance characteristics of a dbt invocation.

Usage

```text
$ dbt run -r timing.txt
...

$ snakeviz timing.txt
```

Alternatively, you can use [`py-spy`](https://github.com/benfred/py-spy) to collect [speedscope](https://github.com/jlfwong/speedscope) profiles of dbt commands like this:

```shell
python -m pip install py-spy
sudo py-spy record -s -f speedscope -- dbt parse
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
