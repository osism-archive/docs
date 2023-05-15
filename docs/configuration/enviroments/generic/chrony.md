# Chrony

Base directory: **environments/generic**

* <https://chrony.tuxfamily.org/>

**Name**                         |**Repository**                                                                |**Documentation**
---------------------------------|------------------------------------------------------------------------------|-----------------
collection.osism.services.chrony |<https://github.com/osism/ansible-collection-services/tree/main/roles/chrony> |---

## Configuration

* Configuration for **chrony** only as Client

```yaml
##########################
# chrony

enable_chrony: true
chrony_servers:
    - 10.0.3.1
    - 10.0.3.2
chrony_allowed_subnets:
    - 127.0.0.1/32
```

* Configure **manager** as NTP client and server and **control** as NTP client

```yaml
##########################
# chrony

enable_chrony: true
chrony_servers:
    - 0.de.pool.ntp.org
    - 1.de.pool.ntp.org
    - 2.de.pool.ntp.org
    - 3.de.pool.ntp.org
chrony_allowed_subnets:
    - 10.0.3.0/24
```

```yaml
##########################
# chrony

enable_chrony: true
chrony_servers:
    - 10.0.3.1 # manager1
    - 10.0.3.2 # manager2
chrony_allowed_subnets:
    - 127.0.0.1/32
```
