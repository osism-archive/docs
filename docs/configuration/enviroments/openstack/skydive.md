# Skydive

:::note

>**note:** Service Skydive is a technical preview.

:::

* <http://skydive.network/documentation/>

## Dependencies

* elasticsearch
* etcd (optional)
* openvswitch (optional)

## Configuration parameters

```yaml
enable_skydive: "yes"
```

## Inventory groups

```ini
[skydive:children]
monitoring

# skydive

[skydive-analyzer:children]
skydive

[skydive-agent:children]
compute
network
```
