# Update

There are three possible ways to update the configuration repository.

On the seed node containing the configuration repository, created by *cookiecutter*, change into **environments/manager**
directory and execute the following command.  This will update the configuration repository on the manager node.

```sh
./run.sh configuration
```

On the manager node use the following command to update the configuration repository.

```sh
osism-manager configuration
```

Alternatively, Git itself can be used on the manager node to update the repository. Therefore the deploy key have to have write
permissions.

```sh
cd /opt/configuration
ssh-agent bash -c 'ssh-add ~/.ssh/id_rsa.configuration; git pull'
```
