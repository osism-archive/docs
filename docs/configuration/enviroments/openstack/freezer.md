# Freezer

:::note

>**note:** Service Freezer is a technical preview.

:::

* <https://docs.openstack.org/freezer/latest/>

## Configuration parameters

```yaml
enable_freezer: "yes"
enable_horizon_freezer: "{{ enable_freezer | bool }}"
```

## Inventory groups

```ini
[freezer:children]
control

# freezer

[freezer-api:children]
freezer

[freezer-scheduler:children]
freezer
```
