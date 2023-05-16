# Networks

![Network Schema](./images/network-schema.png)

**Name**            |**Nodes**                                      |**MTU**|**Optional**|**Routed**
--------------------|-----------------------------------------------|-------|:----------:|:--------:|
Management          |all nodes                                      |1500   |✖️           |✔️
Internal            |all nodes                                      |1500   |✖️           |✖️
Tunnel              |compute & network nodes                        |1500   |✔️           |✖️
Migration           |compute nodes                                  |1500   |✔️           |✖️
External API        |controller nodes                               |1500   |✔️           |N/A
External Networks   |network nodes                                  |1500   |✔️           |✔️
Provider Networks   |compute & network nodes                        |1500   |✔️           |N/A
Ceph Frontend       |all nodes that require access to the storage   |1500   |✖️           |✖️
Ceph Backend        |storage nodes                                  |1500   |✖️           |✖️
