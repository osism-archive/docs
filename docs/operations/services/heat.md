# Heat

* <https://docs.openstack.org/heat/latest/man/heat-manage.html>

## Purge db entries

:::note

>**note:** This command is executed on a controller node.

:::

```sh
docker exec -it heat_api heat-manage purge_deleted -g days 14
```

## Clean dead engine records

:::note

>**note:** This command is executed on a controller node.

:::

```sh
docker exec -it heat_api heat-manage service clean
Dead engines are removed.
```
