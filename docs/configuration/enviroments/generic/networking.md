# Networking

Base directory: **environments/generic**

## Interfaces

**Name**                    |**Repository**                                         |**Documentation**
----------------------------|-------------------------------------------------------|-----------------
osism.network-interfaces    |<https://github.com/osism/ansible-network-interfaces>  |---

## Proxy

**Name**                    |**Repository**                                         |**Documentation**
----------------------------|-------------------------------------------------------|-----------------
osism.proxy                 |<https://github.com/osism/ansible-proxy>               |---

* proxy for manager bootstrap

```yaml
##########################
# proxy

proxy_url_custom: "http://proxy_user:proxy_pwd@proxy_url:proxy_port"
proxy_proxies:
    http: "{{ proxy_url_custom }}"
    https: "{{ proxy_url_custom }}"
proxy_package_manager: no
```

* proxy for operating system in **/etc/environment** and for **apt**.

```yaml
# system proxy
proxy_url_custom: "http://proxy_user:proxy_pwd@proxy_url:proxy_port"
proxy_proxies:
    http: "{{ proxy_url_custom }}"
    https: "{{ proxy_url_custom }}"
proxy_package_manager: no
proxy_no_proxy_extra:
    - 127.0.0.1
    - 10.0.3.10
    - 10.0.3.11
    - 10.0.3.12
    - localhost
    - api-int.osism.xyz
    - api.osism.xyz
```

* proxy for docker to pull images.

```yaml
# docker proxy
proxy_url_custom: "http://proxy_user:proxy_pwd@proxy_url:proxy_port"
docker_configure_proxy: true
docker_proxy_http: "{{ proxy_url_custom }}"
docker_proxy_https: "{{ proxy_url_custom }}"
docker_proxy_no_proxy:
    - 127.0.0.1
    - 10.0.3.10
    - 10.0.3.11
    - 10.0.3.12
    - localhost
    - api-int.osism.xyz
    - api.osism.xyz
```

## Resolvconf

**Name**                    |**Repository**                                         |**Documentation**
----------------------------|-------------------------------------------------------|-----------------
osism.resolvconf            |<https://github.com/osism/ansible-resolvconf>          |---

If available, the company's internal DNS servers should be used. If no own DNS servers are available, we recommend using the DNS
servers from [Quad9](https://www.quad9.net).

* **environments/configuration.yml**

```yaml
##########################
# resolvconf

resolvconf_nameserver:
    - 9.9.9.9
    - 149.112.112.112
resolvconf_search: osism.io
```

## Network Interface Card Offloading

This section only applies if vxlan or geneve tunnels are enabled for overlay networks. To avoid performance issues, you can
enable hardware offloading.

This can verify with ethtool and the following paramameter:

```sh
ethtool -k enoX 
```

This can set with ethtool and following parameters:

```sh
ethtool -K enoX rx on tx on sg on tso on lro on
```

If jumboframes are enabled it make sense to increase the tx and rx buffer to the hardware possible maximum size.

This can verify with ethtool and the following paramameter:

```sh
ethtool -g enoX 
```

This can set with ethtool and following parameters:

```sh
ethtool -G enoX rx <Size> tx <Size> 
```
