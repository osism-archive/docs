# Extra Configuration

This chapter is about some options beyond the basic setup.
Some examples are from firm of production environments.

## Fluentd Authentication

If fluentd is needed to communicate with elasticsearch authenticated or it shall interact with a company independent
elasticsearch, you are able to configure it with the following configuration parameters:

```yaml
fluentd_elasticsearch_user: "<operator>"
elasticsearch_address: "<otherhost>"
```

```yaml
fluentd_elasticsearch_password: "<SomePassword>"
```
