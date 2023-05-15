# Grub

Base directory: **environments/generic**

**Name**    |**Repository**                         |**Documentation**
------------|---------------------------------------|-------------------------------
debops.grub |<https://github.com/debops/debops>     |<https://docs.debops.org/en/master/ansible/roles/grub/defaults/main.html#grub-configuration-options>

## Blacklist a module

```yaml
##########################
# grub

grub__default_configuration:
    - name: 'cmdline_linux_default'
        value:
        - modprobe.blacklist=qla2xxx
```

## Disable predictable network interface names

```yaml
##########################
# grub

grub__default_configuration:
    - name: 'cmdline_linux_default'
        value:
        - net.ifnames=0
        - biosdevname=0
```

## Enable IOMMU

### Intel

```yaml
##########################
# grub

grub__default_configuration:
    - name: 'cmdline_linux_default'
        value:
        - intel_iommu=on
        - iommu=pt
```

### AMD

```yaml
##########################
# grub

grub__default_configuration:
    - name: 'cmdline_linux_default'
        value:
        - iommu=pt
```

## Support of Docker capabilities

:::note

>**note:** Memory and swap accounting incur an overhead of about 1% of the total available memory and a 10% overall performance
>degradation, even if Docker is not running.

:::

```sh
$ docker info
[...]
WARNING: No swap limit support
```

This is actual the default in **debops grub** <https://github.com/debops/debops/blob/master/ansible/roles/grub/defaults/main.yml#L138>

```yaml
##########################
# grub

grub__default_configuration:
    - name: 'cmdline_linux_default'
        value:
        - cgroup_enable=memory
        - swapaccount=1
```
