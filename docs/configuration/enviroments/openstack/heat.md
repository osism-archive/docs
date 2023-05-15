# Heat

* <https://docs.openstack.org/heat/latest/>

## Configuration parameters

```yaml
enable_heat: "yes"
enable_horizon_heat: "{{ enable_heat | bool }}"
```

## Inventory groups

```ini
[heat:children]
control

# heat

[heat-api:children]
heat

[heat-api-cfn:children]
heat

[heat-engine:children]
heat
```

## Use of self-signed certificates

```ini
[clients_keystone]
insecure = True
```
