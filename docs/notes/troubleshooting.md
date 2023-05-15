# Troubleshooting

## Sudo: unknown uid 42401: who are you?

* <https://bugs.launchpad.net/kolla-ansible/+bug/1680139>

Solution: Stop the **nscd** service.

## Refusing to convert from file to symlink for /etc/resolv.conf

```sh
sudo rm /etc/resolv.conf
sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```
