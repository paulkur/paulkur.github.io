---
title: OpenWRT Setup
date: 2023-05-12 12:53:00 +0100
categories: [homelab,hardware,setup,proxmox,openwrt]
tags: [servers,docs,proxmox,setup,openwrt]     # TAG names should always be lowercase
pin: true
---

## Links

- [Links](#links)
  - [OpenWRT Instalation - Setup on x86 PC](#openwrt-instalation---setup-on-x86-pc)
- [NFS Share setup](#nfs-share-setup)
  - [Server configuration](#server-configuration)
  - [Client configuration](#client-configuration)

Useful links:

- [usefull comands](https://www.youtube.com/watch?v=lZjMxdBPH7M&t=917s)
- [Proxmox Setup](https://www.youtube.com/watch?v=GoZaMgEgrHw)

### OpenWRT Instalation - Setup on x86 PC

- Set static IP address

```bash
uci set network.lan.ipaddr='192.168.0.201'
```

- download image

```bash
wget https://downloads.openwrt.org/releases/22.03.5/targets/x86/64/openwrt-22.03.5-x86-64-generic-ext4-combined.img.gz
gunzip openwrt-22.03.5-x86-64-generic-ext4-combined.img.gz
lsblk
sudo mkfs -t ext4 /dev/sdb
# Write image
dd if=openwrt-22.03.5-x86-64-generic-ext4-combined.img bs=1M of=/dev/sdb
parted /dev/sdb print
parted -s /dev/sdb resizepart 2 100%
resize2fs /dev/sdb2
```

- Installing by default

```bash
opkg update && opkg install kmod-pcengines-apuv2 beep kmod-leds-gpio kmod-crypto-hw-ccp kmod-usb-core kmod-usb-ohci kmod-usb2 kmod-usb3 kmod-sound-core kmod-pcspkr amd64-microcode flashrom irqbalance fstrim usbutils curl luci-app-advanced-reboot
```

- For using usb drives

```bash
opkg update && opkg install block-mount e2fsprogs kmod-fs-ext4 kmod-usb-storage kmod-usb2 kmod-usb3 hdparm luci-app-hd-idle gdisk libblkid nfs-kernel-server nano f2fs-tools kmod-fs-f2fs
```

- If you have wle200nx, wle600vx or wle900vx WiFI adapters : hostapd-openssl

```bash
opkg install kmod-ath9k ath9k-htc-firmware ath10k-firmware-qca988x kmod-ath10k hostapd-openssl 
```

- If you have a Quectel 4G modem

```bash
opkg install qmi-utils kmod-usb-net-qmi-wwan libqmi luci-proto-qmi uqmi
```

check connection status

```bash
uqmi -d /dev/cdc-wdm0 --get-data-status
```

check sim status

```bash
qmicli --device=/dev/cdc-wdm0 --device-open-proxy --uim-get-card-status
```

- If you want to run Wireguard

```bash
opkg install luci-proto-wireguard luci-app-wireguard
```

- And here are packages for OpenVPN

```bash
opkg install openvpn-openssl ip-full luci-app-openvpn
```

## NFS Share setup

- config openWRT [instructions Here](https://openwrt.org/docs/guide-user/services/nas/nfs.server)

>NOTE: Don't forget firewall rules ! Important, will not work otherwise.
{: .prompt-warning }

Shares are defined in `/etc/exports`.

```bash
opkg update && opkg install nfs-kernel-server nfs-kernel-server-utils
```

To verify your server is basically working for NFSv4, you can create a loopback setup, using an empty directory on tmpfs:
>NOTE: This destroys the contents of your existing `/etc/exports` file!
{: .prompt-warning }

```bash
# Create a directory and set up exports(5) to share it with the world in read/write mode
mkdir /tmp/nfsdemo/
chmod 1777 /tmp/nfsdemo/
echo '/tmp/nfsdemo/ *(rw,all_squash,insecure,no_subtree_check,fsid=0)' > /etc/exports
exportfs -ra
 
# Verify exporting worked
showmount -e localhost
 
# Mount the exported directory in another directory on the same host via NFSv4
mkdir /tmp/nfsclienttest/
mount -t nfs -o vers=4 localhost:/ /tmp/nfsclienttest/
```

At this point, you should be able to play around with various filesystem-affecting commands (touch, cp, et al.) in both `/tmp/nfstest` (the server view) and `/tmp/nfsclienttest` (the client view) to observe how state in one reflects in the other, and how the consequences of UID squashing over NFS play out.

### Server configuration

Use the file `/etc/exports` to configure your shares. NFSv4 export paths don't work the way they did in NFSv3. NFSv4 has a global root directory (configured as fsid=0) and all exported directories are children to it. So what would have been `nfs-server:/export/users` on NFSv3 is `nfs-server:/users` on NFSv4, because `/export` is the root directory. Example:

```bash
/mnt        *(fsid=0,ro,sync,no_subtree_check)
/mnt/sda1   192.168.1.0/24(rw,sync,no_subtree_check)
/mnt/sda2   192.168.2.0/255.255.255.0(rw,sync,no_subtree_check)
```

See exports(5) for configuration semantics. A single asterisk matches all IP addresses/hosts (allowing anonymous access).

Assuming the daemons are already running, use the command `exportfs -ar` to reload and apply changes on the fly.

```bash
exportfs -ar
```

- Current `/etc/exports` in `fuji`. [Instruction is here](https://openwrt.org/docs/guide-user/services/nas/nfs.server)

```bash
nano /etc/exports
```

```bash
/mnt/backups/ *(rw,all_squash,insecure,no_subtree_check,fsid=0)
/mnt/fujiinside/ *(rw,all_squash,insecure,no_subtree_check,fsid=0)
/mnt/m2drive/ *(rw,all_squash,insecure,no_subtree_check,fsid=0)
```

```bash
exportfs -ra
```

```bash
showmount -e localhost
```

```bash
mount -t nfs -o vers=4 localhost:/ /tmp/nfsclienttest/
```

```bash
mount -t nfs -o vers=4 localhost:/ /mnt/backups/
mount -t nfs -o vers=4 localhost:/ /mnt/fujiinside/
mount -t nfs -o vers=4 localhost:/ /mnt/m2drive/
```

```bash
chmod 1777 /mnt/backups/ && chmod 1777 /mnt/fujiinside/ && chmod 1777 /mnt/m2drive/
```

### Client configuration

Linux-Client

Mount manually:

```bash
sudo mount 192.168.1.254:/sda1 /home/sandra/nfs_share
```

Mount manually curently Linux TurnKey (FS-trnt)

```bash
sudo mount 192.168.1.1:/backups /mnt/pve/backups
sudo mount 192.168.1.1:/fujiinside /mnt/pve/fujiinside
sudo mount 192.168.1.1:/m2drive  /mnt/pve/m2drive
```

Or mount permanently with entries in the `/etc/fstab` on each client PC:

```bash
192.168.1.254:/sda1 /media/openwrt       nfs  ro,async,auto,_netdev  0  0
192.168.1.254:/sda2 /media/remote_stuff  nfs  rw,async,auto,_netdev  0  0
```
