# Seed node

:::note

>**note:** Execute the following commands on the seed node.

:::

The seed node is used once for the first bootstrap of the manager node. It is sufficient to use the local workstation. It doesn't
have to be a dedicated system. The seed node must be able to reach the manager node via SSH.

The use of Linux on the seed node is recommended. Other operating systems should also work without problems.

## Install required packages

Ubuntu/Debian:

```sh
sudo apt install git python3-pip python3-virtualenv sshpass
```

MacOS:

**sshpass** cannot be installed directly as a package via homebrew (**We won't add sshpass because it makes it too easy for
novice SSH users to ruin SSH's security.**).

```sh
brew tap esolitos/ipa
brew install esolitos/ipa/sshpass
```

## Get a copy of the configuration repository

The configuration repository to be used must be available on the seed node. In the following example, replace **ORGANIZATION**
and **cfg-ENVIRONMENT** accordingly.

```sh
git clone ssh://git@github.com:ORGANIZATION/cfg-ENVIRONMENT.git
```

If necessary, the configuration SSH key can be used for the initial transfer of the repository.

For this, the following content is added in **~/.ssh/config** and the SSH privte key is stored in
**~/.ssh/id_rsa.configuration**.

```none
   Host github.com
     HostName github.com
     User git
     Port 22
     IdentityFile ~/.ssh/id_rsa.configuration
```
