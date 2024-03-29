# Preparation of the servers

The manual installation of a system is described below. The use of an installation server is recommended

The manual node installation is completely possible without network connectivity.

## Preparations

* Download the latest ISO image for Ubuntu 20.04 from

  * <http://cdimage.ubuntu.com/ubuntu-legacy-server/releases/20.04/release/>
  * Use the **ubuntu-20.04.1-legacy-server-amd64.iso** image
  * Do not use the **ubuntu-20.04-live-server-amd64.iso** image
  * The version number may be different, always use the latest available version of 20.04 LTS

* Or use the preseed ISO, downloadable at

  * <https://minio.services.osism.tech/manager-installer/osism-manager-installer.iso>
  * ubuntu / ubuntu
  * Use clean, empty disks for this setup (e.g. use dd, shred or remove LVM/RAID configuration manually)

* Create a bootable USB stick from this ISO image. Alternatively you can also work with a CD
* Perform a hardware RAID configuration if necessary, not for Ceph disks.
* Boot bare-metal server via UEFI (recommended) from this USB stick/CD

:::note

>**note:** We prefer the use of software RAIDs to make us less dependent on hardware. But there is nothing against using hardware
>RAIDs.

:::

* Boot bare-metal server from this USB stick/CD

## Manual Installation

* Choose **English** as language
* Choose **Install Ubuntu Server**
* Choose **English** as language (again)
* Choose your location (e.g. **Germany**)
* Choose **en_US.UTF-8** as locale
* Choose the keyboard layout from a list, use **English (US)**
* Choose and configure the primary network interface

  * Depending on the environment, the network may not work at this point. Then select any interface and then select
    **Do not configure the network at this time** in the next step.

* Set the hostname (the hostname is **60-10** and not **60-10.betacloud.xyz**)
* Set **ubuntu** as full name for the new user
* Set **ubuntu** as the username for the account

  * The later used operator user **dragon** is created during the bootstrap and should not be created during the installation.

* Set a password for the account

  * The account is only needed initially and can be deleted after completion of the bootstrap.

* Choose **Manual** as partitioning method and execute the partitioning according to company specifications

  * Details can be found in section [Partitioning](#node-partitioning).

* Choose **No automatic updates**
* Choose **OpenSSH server** as software to install

:::note

>**note:** Do not install any other software component. Everything you need will be installed later by OSISM. In particular, it
>is not necessary to install a desktop environment.

* After completion, restart the system

:::note

>**note:** **python3-minimal** must be installed on the systems.

### Node Partitioning

* The use of a UEFI is recommended
* The use of a RAID is recommended

:::note

>**note:** We prefer the use of software RAIDs to make us less dependent on hardware. But there is nothing against using
>hardware RAIDs.

:::

* The use of a LVM2 is recommended

  * **system** is recommended as the name for the volume group

:::note

>**note:** Dedicated disks may be provided for **/var/lib/docker** on the controller nodes. In this case, do not use an LV for
>**/var/lib/docker** but the devices provided for it.

:::

* Do not configure devices that are not required for the operating system

The use of own file systems for the following mountpoints is recommended. The minimum size and a recommended name for the logical
volume are noted.

* **/** (10 GByte, logical volume **root**)
* **/home** (2 GByte, logical volume **home**)
* **/tmp** (5 GByte, logical volume **tmp**)
* **/var/lib/ceph** (50 GByte, logical volume **ceph**) (optional for storage nodes)
* **/var/lib/docker** (30 GByte, logical volume **docker**, do not set the **nosuid** flag on **/var/lib/docker**)
* **/var/log/audit** (1 GByte, logical volume **audit**)
* **/var** (10 GByte, logical volume **var**)
* **swap** (min 8 GByte, logical volume **swap**)

:::note

>**note:** The size of the individual partitions is minimal. Depending on the node type, the individual partitions should be made
>larger. This applies in particular to **/var/lib/docker**. On controllers at least 100 GByte should be used.
>
>A later enlargement is possible during operation.
>
>```sh
> # lvextend -L +10G /dev/mapper/system-docker
> # resize2fs -p /dev/mapper/system-docker
>```

:::

The following is a sample view from the Ubuntu installer. This view may vary depending on the environment.

![installation-partition-disks](./images/installation-partition-disks.png)

:::note

>**note:** When using XFS as the file system for **/var/lib/docker**, note the following: Running on XFS without d_type support
>now causes Docker to skip the attempt to use the overlay or overlay2 driver.
>
><https://docs.docker.com/storage/storagedriver/overlayfs-driver/>

:::

### Screenshots

Step by step of manual installation with screenshots.

* Boot via CD-ROM/ISO Ubuntu and choose **Install Ubuntu Server**

![Grub](./images/manual-installation/01-grub.png)

* Select language **English**

![Language](./images/manual-installation/02-language.png)

* Select your country, e.g. Europe/Germany

![Country](./images/manual-installation/03-country.png)
![Location](./images/manual-installation/04-location.png)
![Location-2](./images/manual-installation/05-location.png)

* Choose **en_US.UTF-8** as locale

![Locales](./images/manual-installation/06-locales.png)

* Do **not** detect Keyboard layout

![Keyboard-detect](./images/manual-installation/07-keyboard-detect.png)

* Choose Keyboard Country **English (US)**

![Keyboard-select](./images/manual-installation/08-keyboard-select.png)

* Keyboard layout **English (US)**

![Keyboard-layout](./images/manual-installation/09-keyboard-layout.png)

* Choose your Hostname, e.g. 60-10, manager, compute, controller, ctrl, com, sto, ...

![Hostname](./images/manual-installation/10-hostname.png)

* Full name of User, **ubuntu**

![Username-full](./images/manual-installation/11-username-full.png)

* username **ubuntu**

![Username](./images/manual-installation/12-username.png)

* Set password

![Password](./images/manual-installation/13-password.png)
![Password-reenter](./images/manual-installation/14-password-reenter.png)

* Set Timezone, e.g. **Europe/Berlin**

![Timezone](./images/manual-installation/15-timezone.png)

* Partitioning - Choose the **Guided - use entire disk and set up LVM** entry

![Partition](./images/manual-installation/16-partition.png)

* Choose the first disk

![Partition](./images/manual-installation/17-partition.png)

* Write the changes to disk

![Partition](./images/manual-installation/18-partition.png)

* Continue with the suggested value

![Partition](./images/manual-installation/19-partition.png)

* **Configure the Logical Volume Manager**

![Partition](./images/manual-installation/20-partition.png)

* Write the changes to disk

![Partition](./images/manual-installation/21-partition.png)

* Delete all suggested Logical Volumes

![Partition](./images/manual-installation/22-partition.png)
![Partition](./images/manual-installation/23-partition.png)

* Create LVs like here [Partitioning](#node-partitioning) with ext4

![Partition](./images/manual-installation/24-partition.png)
![Partition](./images/manual-installation/25-partition.png)
![Partition](./images/manual-installation/26-partition.png)
![Partition](./images/manual-installation/27-partition.png)
![Partition](./images/manual-installation/28-partition.png)
![Partition](./images/manual-installation/29-partition.png)
![Partition](./images/manual-installation/30-partition.png)
![Partition](./images/manual-installation/31-partition.png)
![Partition](./images/manual-installation/32-partition.png)
![Partition](./images/manual-installation/33-partition.png)
![Partition](./images/manual-installation/34-partition.png)

* For **swap** LV use **swap area**

![Partition-Swap](./images/manual-installation/35-partition-swap.png)
![Partition-Swap](./images/manual-installation/36-partition-swap.png)

* The partitioning should look like this

![Partition](./images/manual-installation/37-partition.png)

* Write the changes to disk

![Partition](./images/manual-installation/38-partition.png)

* Installation will be started

![Installation](./images/manual-installation/39-installation.png)

* Proxy?

![Proxy](./images/manual-installation/40-proxy.png)
![Installation](./images/manual-installation/41-installation.png)

* Choose **No automatic updates**

![Autoupdate](./images/manual-installation/42-autoupdate.png)

* Choose **OpenSSH server** to install

![Openssh](./images/manual-installation/43-openssh.png)
![Installation](./images/manual-installation/44-installation.png)

* After finished installation, choose **Continue** for reboot

![Complete](./images/manual-installation/45-complete.png)

* After reboot the installed Grub looks like this

![Installed Grub](./images/manual-installation/46-installed-grub.png)

* Finaly the login prompt appears

![Installed Prompt](./images/manual-installation/47-installed-prompt.png)

## Preseed Installation

### Prepare Ubuntu Server ISO

* Prepare your environment as root

```sh
mkdir /dev/shm/ubuntu-seed
sudo mount -o loop,ro ubuntu-20.04-legacy-server-amd64.iso /mnt/
cp -rT /mnt /dev/shm/ubuntu-seed
```

* Edit in both files the first entry as **root**

```sh
vim boot/grub/grub.cfg
```

```cfg
menuentry "Install Ubuntu Server OSISM" {
   set gfxpayload=keep
   linux  /install/vmlinuz auto console-setup/ask_detect=false console-setup/layoutcode=us console-setup/modelcode=pc105 debconf/frontend=noninteractive debian-installer=en_US.UTF-8 fb=false initrd=/install/initrd.gz kbd-chooser/method=us keyboard-configuration/layout=USA keyboard-configuration/variant=USA locale=en_US.UTF-8 noapic preseed/file=/cdrom/preseed/osism-ubuntu-server.seed ---
   initrd /install/initrd.gz
}
```

```sh
vim isolinux/txt.cfg
```

```cfg
label install
    menu label ^Install Ubuntu Server OSISM
    kernel /install/vmlinuz
    append auto console-setup/ask_detect=false console-setup/layoutcode=us console-setup/modelcode=pc105 debconf/frontend=noninteractive debian-installer=en_US.UTF-8 fb=false initrd=/install/initrd.gz kbd-chooser/method=us keyboard-configuration/layout=USA keyboard-configuration/variant=USA locale=en_US.UTF-8 noapic preseed/file=/cdrom/preseed/osism-ubuntu-server.seed vga=788 initrd=/install/initrd.gz ---
```

:::note

>**note:** Please use **:w!** in vim for writing readonly files

* Create preseed file, [Preseed file](#preseed-file).

```sh
cat preseed/osism-ubuntu-server.seed
### Localization

# Preseeding language, country and locale
d-i debian-installer/locale string en_US.UTF-8
...
### Boot loader installation

d-i grub-installer/grub2_instead_of_grub_legacy boolean true
d-i grub-installer/only_debian boolean false
d-i grub-installer/with_other_os boolean true
d-i grub-installer/bootdev string default
d-i grub-installer/timeout string 5
# Avoid that last message about the install being complete.
d-i finish-install/reboot_in_progress note
```

* Write new md5sum in reference file, md5sum.txt

```sh
$ md5sum boot/grub/grub.cfg
39c2565e2d6eff27b806f0b41382db66  boot/grub/grub.cfg
$ grep grub.cfg md5sum.txt
...
39c2565e2d6eff27b806f0b41382db66  ./boot/grub/grub.cfg

$ md5sum preseed/osism-ubuntu-server.seed
09361c56b41e218df314478947491cb3  preseed/osism-ubuntu-server.seed
$ grep osism md5sum.txt
09361c56b41e218df314478947491cb3  ./preseed/osism-ubuntu-server.seed
```

* Build ISO file

```sh
$ mkisofs -U -A "UbuntuOSISM" -V "UbuntuOSISM" -volset "UbuntuOSISM" -J -joliet-long -r -v \
-T -o /path/to/osism-ubuntu-seed.iso -b isolinux/isolinux.bin -c isolinux/boot.cat \
-no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e boot/grub/efi.img \
-no-emul-boot /dev/shm/ubuntu-seed/
```

:::note

>**note:**
>
>* Please use console, ALT+F4, for debugging
>* UEFI boot only
>* please use disk size minimum of 63GB (10 + 2 + 2 + 30 + 1 + 10 + 8, see partitioning above), otherwise the default LVs will be
>  active, root/swap

:::

## Post-processing

### EFI partitions

* <https://askubuntu.com/questions/1066028/install-ubuntu-18-04-desktop-with-raid-1-and-lvm-on-machine-with-uefi-bios>

```sh
lsblk
```

```none
NAME                MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                   8:0    0 59.6G  0 disk
├─sda1                8:1    0  476M  0 part  /boot/efi
└─sda2                8:2    0 59.2G  0 part
    └─md0               9:0    0 59.1G  0 raid1
    ├─system-root   253:0    0  9.3G  0 lvm   /
    ├─system-swap   253:1    0  7.5G  0 lvm   [SWAP]
    ├─system-tmp    253:2    0  1.9G  0 lvm   /tmp
    ├─system-audit  253:3    0  952M  0 lvm   /var/log/audit
    ├─system-var    253:4    0  9.3G  0 lvm   /var
    ├─system-docker 253:5    0  9.3G  0 lvm   /var/lib/docker
    └─system-home   253:6    0  1.9G  0 lvm   /home
sdb                   8:16   0 59.6G  0 disk
├─sdb1                8:17   0  476M  0 part
└─sdb2                8:18   0 59.2G  0 part
    └─md0               9:0    0 59.1G  0 raid1
    ├─system-root   253:0    0  9.3G  0 lvm   /
    ├─system-swap   253:1    0  7.5G  0 lvm   [SWAP]
    ├─system-tmp    253:2    0  1.9G  0 lvm   /tmp
    ├─system-audit  253:3    0  952M  0 lvm   /var/log/audit
    ├─system-var    253:4    0  9.3G  0 lvm   /var
    ├─system-docker 253:5    0  9.3G  0 lvm   /var/lib/docker
    └─system-home   253:6    0  1.9G  0 lvm   /home
```

```sh
dd if=/dev/sda1 of=/dev/sdb1
```

```sh
efibootmgr -v | grep ubuntu
```

```none
Boot0000* ubuntu    HD(1,GPT,f6b80cef-a636-439a-b2c2-e30bc385eada,0x800,0xee000)/File(\EFI\UBUNTU\SHIMX64.EFI)
Boot0018* ubuntu    HD(1,GPT,f6b80cef-a636-439a-b2c2-e30bc385eada,0x800,0xee000)/File(\EFI\UBUNTU\GRUBX64.EFI)
```

```sh
efibootmgr -c -d /dev/sdb -p 1 -L "ubuntu2" -l "\EFI\UBUNTU\GRUBX64.EFI"
efibootmgr -c -d /dev/sdb -p 1 -L "ubuntu2" -l "\EFI\UBUNTU\SHIMX64.EFI"
```

### Network

After the first boot depending on the environment it is necessary to create the network configuration for the management
interface manually, because for example bonding or VLANs should be used.

The following examples shows how the configuration can be done with **netplan** or **iproute2**.

:::note

>**note:** The examples are not the final network configuration. It is a minimal sample network configuration for initial access
>to the systems.
>
>The example configuration differs depending on the environment. The configuration is not a recommendation for the network
>design. It's just an example configuration.
>
>It is not necessary to manually create the finale network configuration. The final network configuration of the environment is
>defined during the creation of the configuration repository. The network final network configuration is depoyed during the
>bootstrap on the systems.

#### iproute2

* <https://baturin.org/docs/iproute2/>
* <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-vlan_on_bond_and_bridge_using_ip_commands>
* <https://www.kernel.org/doc/Documentation/networking/bonding.txt>

```sh
modprobe bonding
ip link add bond0 type bond
ip link set bond0 type bond miimon 100 mode 802.3ad lacp_rate 1
ip link set eno1 down
ip link set eno1 master bond0
ip link set eno2 down
ip link set eno2 master bond0
ip link set bond0 up
cat /proc/net/bonding/bond0
```

```sh
ip link add link bond0 name vlan101 type vlan id 101
ip link set vlan101 up
```

```sh
ip address add 172.17.60.10/16 dev vlan101
ip route add default via 172.17.40.10
```

* You may have to set the nameservers in **/etc/resolv.conf**. Temporarily remove the **127.0.0.53** entry.

#### Netplan

* <https://netplan.io/examples>
* configure **/etc/netplan/01-netcfg.yaml**

```yaml
---
network:
    version: 2
    renderer: networkd
    ethernets:
    eno1:
    dhcp4: no
    eno2:
    dhcp4: no
    bonds:
    bond0:
    dhcp4: no
    interfaces:
    - eno1
    - eno2
    parameters:
    mode: 802.3ad
    lacp-rate: fast
        mii-monitor-interval: 100
    vlans:
    vlan101:
    id: 101
    link: bond0
    addresses: [ "172.17.60.10/16" ]
    routes:
    - to: 0.0.0.0/0
    via: 172.17.40.10
    nameservers:
    search: [ betacloud.xyz ]
    addresses: [ "8.8.8.8", "8.8.4.4" ]
```

```sh
netplan apply
```

### ACPI Error

If you see this messages in **dmesg**, logs or **journal**

```sh
ACPI Error: SMBus/IPMI/GenericSerialBus write requires Buffer of length 66, found length 32 (20150930/exfield-418)
ACPI Error: Method parse/execution failed [\_SB.PMI0._PMM] (Node ffff8807ff5bd438), AE_AML_BUFFER_LIMIT (20150930/psparse-542)
ACPI Exception: AE_AML_BUFFER_LIMIT, Evaluating _PMM (20150930/power_meter-338)
```

blacklist and unload kernel module **acpi_power_meter**.

* <https://access.redhat.com/solutions/48109>

## Preseed file

```sh
### Localization

# Preseeding language, country and locale
d-i debian-installer/locale string en_US.UTF-8

# Keyboard selection

# Disable automatic (interactive) keymap detection.
d-i console-setup/ask_detect boolean false
d-i keyboard-configuration/xkb-keymap string us

### Network configuration

# Skip network configuration
d-i netcfg/enable boolean false
# Set hostname and domain
d-i netcfg/get_hostname string ubuntu-host
d-i netcfg/get_domain string osism.customer
# Disable that annoying WEP key dialog.
d-i netcfg/wireless_wep string

### Missing drivers and firmware

d-i hw-detect/load_firmware boolean true

### Mirror
d-i mirror/http/proxy string

### Account setup

# Skip creation of a root account
d-i passwd/root-login boolean false
d-i passwd/make-user boolean true
# User ubuntu with password
d-i passwd/user-fullname string ubuntu
d-i passwd/username string ubuntu
# Normal user's password
d-i passwd/user-password password ubuntu
d-i passwd/user-password-again password ubuntu
d-i user-setup/encrypt-home boolean false
# The installer will not warn about weak passwords.
d-i user-setup/allow-password-weak boolean true

### Clock and time zone setup

# Set hardware clock to UTC.
d-i clock-setup/utc boolean true
# Europe/Berlin
d-i time/zone select Europe/Berlin
# No NTP during installation
d-i clock-setup/ntp boolean false

### Partitioning

#d-i partman-auto/disk string /dev/sda
# Choose LVM
d-i partman-auto/method string lvm
# Remove pre-existing LVM
d-i partman-lvm/device_remove_lvm boolean true
# Remove pre-existing software RAID array
d-i partman-md/device_remove_md boolean true
# Confirm to write the lvm partitions
d-i partman-lvm/confirm boolean true
d-i partman-lvm/confirm_nooverwrite boolean true
# Select the whole disk
d-i partman-auto-lvm/guided_size string max
d-i partman-auto-lvm/new_vg_name string system
d-i partman-partitioning/confirm_write_new_label boolean true
d-i partman/choose_partition select Finish
d-i partman/confirm_nooverwrite boolean true
d-i partman/confirm boolean true
d-i partman-auto/expert_recipe string     \
efi-host-vg ::                            \
    512 512 512 fat32                       \
    $defaultignore{ }                     \
    $reusemethod{ }                       \
    method{ efi }                         \
    format{ }                             \
    .                                     \
    10240 1000 10240 ext4                   \
    $lvmok{ }                             \
    lv_name{ root }                       \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{ ext4 }  \
    mountpoint{ / }                       \
    .                                     \
    2048 1000 2048 ext4                     \
    $lvmok{ }                             \
    lv_name{ home }                       \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{ ext4 }  \
    mountpoint{ /home }                   \
    .                                     \
    5120 1000 5120 ext4                     \
    $lvmok{ }                             \
    lv_name{ tmp }                        \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{ ext4 }  \
    mountpoint{ /tmp }                    \
    .                                     \
    30720 2000 30720 ext4                   \
    $lvmok{ }                             \
    lv_name{ docker }                     \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{ ext4 }  \
    mountpoint{ /var/lib/docker }         \
    .                                     \
    1024 2000 1024 ext4                     \
    $lvmok{ }                             \
    lv_name{ audit }                      \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{ ext4 }  \
    mountpoint{ /var/log/audit }          \
    .                                     \
    10240 3000 10240 ext4                   \
    $lvmok{ }                             \
    lv_name{ var }                        \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{ ext4 }  \
    mountpoint{ /var }                    \
    .                                     \
    8192 3000 8192 swap                     \
    $lvmok{ }                             \
    lv_name{ swap }                       \
    method{ swap } format{ }              \
    use_filesystem{ } filesystem{ swap }  \
    .                                     \
    512 5000 8000000000000 ext4             \
    $lvmok{ }                             \
    lv_name{ placeholder }                \
    method{ lvm } format{ }               \
    use_filesystem{ } filesystem{  }      \
    .

### Apt setup

# Repositories
d-i apt-setup/restricted boolean true
d-i apt-setup/universe boolean true
d-i apt-setup/backports boolean true

### Package selection

tasksel tasksel/first multiselect standard, lubuntu-desktop
# Individual additional packages to install
d-i pkgsel/include string openssh-server python3 htop vim
# No update during installation
d-i pkgsel/upgrade select none
# Language pack selection
d-i pkgsel/language-packs multiselect en
# No language support packages
d-i pkgsel/install-language-support boolean false
# No automatic updates
d-i pkgsel/update-policy select none
# Verbose output and no boot splash screen
d-i debian-installer/quiet  boolean false
d-i debian-installer/splash boolean true

### Boot loader installation

d-i grub-installer/grub2_instead_of_grub_legacy boolean true
d-i grub-installer/only_debian boolean false
d-i grub-installer/with_other_os boolean true
d-i grub-installer/bootdev string default
d-i grub-installer/timeout string 5
# Avoid that last message about the install being complete.
d-i finish-install/reboot_in_progress note
```
