# Cinder

## LVM support

* **environments/kolla/configuration.yml**

```yaml
enable_cinder_backend_lvm: "yes"
```

* create PV and VG

```sh
pvcreate /dev/sdX /dev/sdY
vgcreate cinder-volume /dev/sdX /dev/sdY
```

## iSCSI support

* **environments/kolla/configuration.yml**

```yaml
enable_cinder_backend_iscsi: "yes"
enable_cinder_backend_lvm: "no"
```

* **inventory/hosts**

```ini
[iscsid:children]
compute
storage
ironic-conductor

[multipathd:children]
compute
storage

[tgtd:children]
storage
```

## Luks Encryption

For a volume type witch use Luks Encryption by libvirt it is required to enable it in cinder config in section
**environments/kolla/files/overlays/cinder.conf**.

```ini
[[key_manager]
backend = barbican
```
