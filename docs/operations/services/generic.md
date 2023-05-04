# Generic

## Run commands

```sh
osism-ansible generic all -m shell -a date
testbed-node-0.osism.local | SUCCESS | rc=0 >>
Sun Dec  2 10:28:42 UTC 2018

testbed-node-1.osism.local | SUCCESS | rc=0 >>
Sun Dec  2 10:28:42 UTC 2018
[...]
```

## Force NTP sync

```sh
sudo chronyc -a 'burst 4/4'
200 OK
200 OK
$ sudo chronyc -a makestep
200 OK
200 OK
```

## Check if reboot required

:::note

>**note:** Run this command on the manager node.

:::

```sh
osism-generic check-reboot

PLAY [Check if system reboot is required] **************************************

TASK [Check if /var/run/reboot-required exist] *********************************
ok: [testbed-manager.osism.local]
[...]

TASK [Print message if /var/run/reboot-required exist] *************************
ok: [testbed-manager.osism.local] => {
    "msg": "Reboot of testbed-manager.osism.local required"
}
[...]

PLAY RECAP *********************************************************************
testbed-manager.osism.local        : ok=2    changed=0    unreachable=0    failed=0
[...]
```

## Reboot a system

:::note

>**note:** Run this command on the manager node.

:::

```sh
osism-generic reboot --limit testbed-node-0.osism.local

PLAY [Reboot systems] **********************************************************

TASK [Reboot system] ***********************************************************
changed: [testbed-node-0.osism.local]

PLAY RECAP *********************************************************************
testbed-node-0.osism.local        : ok=1    changed=1    unreachable=0    failed=0
```

## Upgrade packages

:::note

>**note:** Run this command on the manager node.

:::

```sh
osism-generic upgrade-packages
PLAY [Upgrade packages] ********************************************************

TASK [Update package cache] ****************************************************
ok: [testbed-node-0.osism.local]

TASK [Upgrade packages] ********************************************************
ok: [1testbed-node-0.osism.local]

TASK [Remove useless packages from the cache] **********************************
ok: [testbed-node-0.osism.local]

TASK [Remove dependencies that are no longer required] *************************
ok: [testbed-node-0.osism.local]
[...]

PLAY RECAP *********************************************************************
testbed-node-0.osism.local        : ok=4    changed=0    unreachable=0    failed=0
[...]
```

## Cronjobs

Cronjobs are managed in playbook **playbook-cronjobs.yml** in environment **custom**.

* <https://docs.ansible.com/ansible/latest/modules/cron_module.html>

The playbook can be rolled out with **osism-run custom cronjobs**.

Examples can be found in the [cookiecutter repository](https://github.com/osism/cfg-cookiecutter/blob/master/cfg-%7B%7Bcookiecutter.project_name%7D%7D/environments/custom/playbook-cronjobs.yml).

## sosreport

Sos is an extensible, portable, support data collection tool primarily aimed at Linux distributions and other UNIX-like operating
systems.

* <https://github.com/sosreport/sos>

To collect reports from all systems, execute the following command on the manager node.

```sh
osism-generic sosreport
```

The collected reports can be found on the manager node under **/opt/archive/sosreport**. Per system and day there is a tarball
with the corresponding MD5 checksum.

:::note

>**note:** Currently only one run per day is possible.

:::

Currently the following plugins are activated.

* apt
* auditd
* block
* devices
* docker
* dpkg
* filesys
* general
* hardware
* kernel
* kvm
* last
* md
* memory
* networking
* pci
* process
* processor
* python
* services
* ssh
* system
* systemd
* ubuntu
* udev
* usb
* xfs

## Update rsyslog configuration

```sh
osism-generic common --skip-tags always --tags logging
```
