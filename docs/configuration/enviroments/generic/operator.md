# Operator

Base directory: **environments/generic**

**Name**        |**Repository**                              |**Documentation**
----------------|--------------------------------------------|------------------
osism.operator  |<https://github.com/osism/ansible-operator> |---

## Authorized keys

Role **osism.operator** supports the configuration of arbitrary SSH public keys for the operator account.

:::note

>**note:** For logging into the systems, the use of personalized accounts is recommended.

:::

Set the **operator_authorized_keys** list parameter in the **environments/configuration.yml** file.

```yaml
operator_authorized_keys:
    - "ssh-rsa [...]"
    - "ssh-rsa [...]"
    [...]
```

The SSH private key of the operator user, which is used by the Ansible playbooks, is defined in the **environments/secrets.yml**
file via the **operator_private_key** parameter. The associated SSH public key must also be added to the
**operator_authorized_keys** list parameter.

Newly added SSH public keys could be transferred to the systems via **osism-generic operator**.

## Password

```sh
$ mkpasswd --method=sha-512 -- password
$6$Ar5mq/3125X$AIVfpZeKI8v7SiXQYH4v3nVTnyb8eT6oQ3aYAZfFm7Fx9Dmqmb9SEzCwiIuCGowgqdNGORcZq3dH9ILDFiF7U0
```

* **environments/secrets.yml**

```yaml
##########################
# passwords

operator_password: "$6$Ar5mq/3125X$AIVfpZeKI8v7SiXQYH4v3nVTnyb8eT6oQ3aYAZfFm7Fx9Dmqmb9SEzCwiIuCGowgqdNGORcZq3dH9ILDFiF7U0"
```
