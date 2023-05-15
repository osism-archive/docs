# Configuration

Base directory: **environments/generic**

**Name**            |**Repository**                                     |**Documentation**
--------------------|---------------------------------------------------|-----------------
osism.configuration |<https://github.com/osism/ansible-configuration>   |---

## Git

* **environments/configuration.yml**

```yaml
##########################
# configuration

configuration_directory: /opt/configuration
configuration_type: git

configuration_git_host: github.com
configuration_git_port: 22

configuration_git_private_key_file: ~/.ssh/id_rsa.configuration
configuration_git_protocol: ssh
configuration_git_repository: ORGANIZATION/cfg-ENVIRONMENT.git
configuration_git_username: git
configuration_git_version: master
```

### Deploy key

* <https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys>
* <https://docs.gitlab.com/ee/ssh/#per-repository-deploy-keys>

* **environments/secrets.yml**

    ```yaml
    ##########################
    # private ssh keys

    configuration_git_private_key: |
    -----BEGIN RSA PRIVATE KEY-----
    [...]
    -----END RSA PRIVATE KEY-----
    ```
