# Common

Base directory: **environments/generic**

**Name**        |**Repository**                             |**Documentation**
----------------|-------------------------------------------|-----------------
osism.common    |<https://github.com/osism/ansible-common>  |---

## Microcode installation

The parameter **install_microcode_package_common** can be used to install the packages **intel-microcode** and
**amd64-microcode**.

* **environments/configuration.yml**

    ```yaml
    ##########################
    # common

    install_microcode_package_common: yes
    ```

## Hardware clock synchronisation

If a node cannot access the hardware clock, it is possible to deactivate hardware clock synchronisation.

* **environments/manager/host_vars/{{HOSTNAME}}.yml**

```yaml
##########################
# common

systohc_common: false
```
