# Placement

## Placement Resource Query

```sh
openstack --os-cloud admin allocation candidate list \
          --os-placement-api-version 1.10 \
          --resource VCPU='3'
```

```sh
+---+------------+--------------------------------------+-------------------------+
| # | allocation | resource provider                    | inventory used/capacity |
+---+------------+--------------------------------------+-------------------------+
| 1 | VCPU=3     | 33c86ab1-b9ac-4908-be4a-968f1b6a830f | VCPU=0/256              |
+---+------------+--------------------------------------+-------------------------+
```

```sh
openstack --os-cloud admin allocation candidate list \
          --os-placement-api-version 1.10 \
          --resource MEMORY_MB='2048'
```

```sh
+---+----------------+--------------------------------------+-------------------------+
| # | allocation     | resource provider                    | inventory used/capacity |
+---+----------------+--------------------------------------+-------------------------+
| 1 | MEMORY_MB=2048 | 33c86ab1-b9ac-4908-be4a-968f1b6a830f | MEMORY_MB=0/95607       |
+---+----------------+--------------------------------------+-------------------------+
```

```sh
openstack --os-cloud admin allocation candidate list \
          --os-placement-api-version 1.10 \
          --resource DISK_GB='10'
```

```sh
+---+------------+--------------------------------------+-------------------------+
| # | allocation | resource provider                    | inventory used/capacity |
+---+------------+--------------------------------------+-------------------------+
| 1 | DISK_GB=10 | 33c86ab1-b9ac-4908-be4a-968f1b6a830f | DISK_GB=0/115           |
+---+------------+--------------------------------------+-------------------------+
```

```sh
openstack --os-cloud admin resource provider list
```

```sh
+--------------------------------------+---------------------------+------------+
| uuid                                 | name                      | generation |
+--------------------------------------+---------------------------+------------+
| 33c86ab1-b9ac-4908-be4a-968f1b6a830f | node-1.osism.io           |          2 |
+--------------------------------------+---------------------------+------------+
```

```sh
openstack --os-cloud admin resource provider usage show 33c86ab1-b9ac-4908-be4a-968f1b6a830f
```

```sh
+----------------+-------+
| resource_class | usage |
+----------------+-------+
| VCPU           |     0 |
| MEMORY_MB      |     0 |
| DISK_GB        |     0 |
+----------------+-------+
```

### Other Resources like for Baremetal Deployment (Ironic/Bifrost)

```sh
openstack --os-cloud admin allocation candidate list \
          --os-placement-api-version 1.10 \
          --resource CUSTOM_BAREMETAL_RESOURCE_CLASS='1'
```

```sh
+---+-----------------------------------+--------------------------------------+-------------------------------------+
| # | allocation                        | resource provider                    | inventory used/capacity             |
+---+-----------------------------------+--------------------------------------+-------------------------------------+
| 1 | CUSTOM_BAREMETAL_RESOURCE_CLASS=1 | c26a4d66-3e85-4b60-b4bc-734ec15ee745 | CUSTOM_BAREMETAL_RESOURCE_CLASS=0/1 |
+---+-----------------------------------+--------------------------------------+-------------------------------------+
```

## Set Resource

```sh
openstack --os-cloud admin resource provider inventory class set \
          --reserved 0 \
          --total 1 \
          --max_unit 1 \
          4a9da83a-c25b-41dd-80fb-53b5fae80eac \
          CUSTOM_BAREMETAL_RESOURCE_CLASS
```

```sh
+------------------+-------+
| Field            | Value |
+------------------+-------+
| allocation_ratio | 1.0   |
| min_unit         | 1     |
| max_unit         | 1     |
| reserved         | 0     |
| step_size        | 1     |
| total            | 1     |
+------------------+-------+
```
