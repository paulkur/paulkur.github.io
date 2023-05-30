---
title: Proxmox setup
date: 2023-05-13 12:53:00 +0100
categories: [homelab,hardware,setup,proxmox]
tags: [servers,docs,proxmox,setup]     # TAG names should always be lowercase
pin: true
---

## Links

- [Links](#links)
  - [Proxmox Setup](#proxmox-setup)
- [Cloud-init setup](#cloud-init-setup)
- [Terraform setup terraform proxmox providers](#terraform-setup-terraform-proxmox-providers)
  - [NFS Share setup](#nfs-share-setup)
    - [Client configuration](#client-configuration)
- [Extras](#extras)

Useful links:

- [usefull comands](https://www.youtube.com/watch?v=lZjMxdBPH7M&t=917s)
- [Proxmox Setup](https://www.youtube.com/watch?v=GoZaMgEgrHw)

### Proxmox Setup

- copyssh key

```bash
ssh-copy-id -i ~/.ssh/id_rsa root@192.168.1.142
```

- non production updates

```bash
nano /etc/apt/sources.list
```

```bash
## PVE pve-no-subscription repository provided by proxmox.com, NOT recommended for production use
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

- comment out line:

```bash
nano /etc/apt/sources.list.d/pve-enterprise.list
```

- Update all

```bash
apt-get update && apt dist-upgrade
pveam update && reboot
```

{% include embed/youtube.html id='GoZaMgEgrHw' %}

- Proxmox Dark

```bash
bash <(curl -s https://raw.githubusercontent.com/Weilbyte/PVEDiscordDark/master/PVEDiscordDark.sh ) install
```

## Cloud-init setup

{% include embed/youtube.html id='shiIi38cJe4&t=333s' %}

- Ubuntu get cloud image

```bash
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```

- can do ... or better later using ansible

```bash
virt-edit -a jammy-server-cloudimg-amd64.img /etc/ssh/ssh_config
```

change to:

```text
PermitRootLogin Yes
PasswordAuthentication yes
```

- Create VM template

```bash
qm create 9000 --memory 2048 --core 2 --name ubuntu-cloud --net0 virtio,bridge=vmbr0
qm importdisk 9000 jammy-server-cloudimg-amd64.img local-lvm
qm set 9000 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-9000-disk-0
qm set 9000 --ide2 local-lvm:cloudinit
qm set 9000 --boot c --bootdisk scsi0
qm set 9000 --serial0 socket --vga serial0
qm template 9000
qm clone 9000 131 --name yoshi --full
```

```bash
qm create 9000 --memory 2048 --net0 virtio,bridge=vmbr0 --scsihw virtio-scsi-pci
qm set 9000 --scsi0 local-lvm:0,import-from=jammy-server-cloudimg-amd64.img
```

- Rocky WSL init

```bash
# for WSL need to run this
sudo apt-get install linux-image-generic
# Install libguestfs
apt-get install libguestfs-tools
export EDITOR=vi # Set the Editor
printenv | grep EDITOR # Verify
```

Rocky Cloud image

```bash
wget https://download.rockylinux.org/pub/rocky/9/images/x86_64/Rocky-9-GenericCloud-Base.latest.x86_64.qcow2
```

Modify image

```bash
virt-edit -a Rocky-9-GenericCloud-Base.latest.x86_64.qcow2 /etc/cloud/cloud.cfg
# change to 0
disable_root: 0
#add modules
cloud_init_modules:
 - qemu-guest-agent
 - nano
 - wget
 - curl
 - net-tools
```

Create Template

```bash
virt-edit -a Rocky-9-GenericCloud-Base.latest.x86_64.qcow2 /etc/ssh/ssh_config
# change to
PermitRootLogin Yes
PasswordAuthentication yes
# make  template
qm create 9000 --memory 1024 --core 2 --name rocky-cloud --net0 virtio,bridge=vmbr0
qm importdisk 9000 Rocky-9-GenericCloud-Base.latest.x86_64.qcow2 local-lvm
qm set 9000 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-9000-disk-0
qm set 9000 --ide2 local-lvm:cloudinit
qm set 9000 --boot c --bootdisk scsi0
qm set 9000 --serial0 socket --vga serial0
qm template 9000
qm clone 9000 141 --name toshi --full
```

## Terraform setup [terraform proxmox providers](https://registry.terraform.io/providers/Telmate/proxmox/latest/docs)

- terraform add new proxmox user

```bash
pveum role add TerraformProv -privs "Datastore.AllocateSpace Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.Monitor VM.PowerMgmt"
pveum user add terraform-prov@pam --password natoMagic7812
pveum aclmod / -user terraform-prov@pam -role TerraformProv
```

- Create a new role for the future terraform user

```bash
pveum role add TerraformProv -privs "Datastore.AllocateSpace Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.Monitor VM.PowerMgmt"
```

- Create the user "terraform-prov@pve"

```bash
pveum user add terraform-prov@pam --password <password>
```

- Add the TERRAFORM-PROV role to the terraform-prov user

```bash
pveum aclmod / -user terraform-prov@pam -role TerraformProv
```

- After the role is in use, if there is a need to modify the privileges, simply issue the command showed, adding or removing privileges as needed.

```bash
pveum role modify TerraformProv -privs "Datastore.AllocateSpace Datastore.Audit Pool.Allocate Sys.Audit Sys.Console Sys.Modify VM.Allocate VM.Audit VM.Clone VM.Config.CDROM VM.Config.Cloudinit VM.Config.CPU VM.Config.Disk VM.Config.HWType VM.Config.Memory VM.Config.Network VM.Config.Options VM.Migrate VM.Monitor VM.PowerMgmt"
```

- VMS-qemu template proxmox from terraform.io

```bash
resource "proxmox_vm_qemu" "resource-name" {
  name        = "VM-name"
  target_node = "Node to create the VM on"
  iso         = "ISO file name"

  ### or for a Clone VM operation
  # clone = "template to clone"

  ### or for a PXE boot VM operation
  # pxe = true
  # boot = "scsi0;net0"
  # agent = 0
}
```

### NFS Share setup

#### Client configuration

[PROXMOX Mount Drives](https://www.youtube.com/watch?v=lZjMxdBPH7M&t=917s)

- Mount USB disk

```bash
lsblk
lsusb
fdisk -l
sudo mkfs -t ext4 /dev/sdb1
mkdir /mnt/usb-drive

# get UUID 
ls -l /dev/disk/by-uuid/*
nano /etc/fstab
## add line
/dev/disk/by-uuid/DISK_UUID  /mnt/m2drive  exfat    defaults    0
df -h
mount -a
```

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

Step 3: Configure Directory Permissions [Full instructions here](https://operavps.com/docs/install-nfs-on-ubuntu/)

```bash
mkdir -p /mnt/internalssd/torrent/Appz && mkdir -p /mnt/internalssd/torrent/movies && mkdir -p /mnt/internalssd/torrent/books && mkdir -p /mnt/internalssd/torrent/others && mkdir -p /mnt/internalssd/torrent/tutorials && mkdir -p /mnt/internalssd/torrent/music
```

```bash
chown -R nobody:nogroup /mnt/internalssd/torrent/Appz && chown -R nobody:nogroup /mnt/internalssd/torrent/movies && chown -R nobody:nogroup /mnt/internalssd/torrent/books && chown -R nobody:nogroup /mnt/internalssd/torrent/others && chown -R nobody:nogroup /mnt/internalssd/torrent/tutorials && chown -R nobody:nogroup /mnt/internalssd/torrent/music
```

```bash
chmod 777 /mnt/internalssd/torrent/Appz && chmod 777 /mnt/internalssd/torrent/movies && chmod 777 /mnt/internalssd/torrent/books && chmod 777 /mnt/internalssd/torrent/others && chmod 777 /mnt/internalssd/torrent/tutorials && chmod 777 /mnt/internalssd/torrent/music
```

```bash
chown -R nobody:nogroup /mnt/backups && chown -R nobody:nogroup /mnt/fujiinside && chown -R nobody:nogroup /mnt/m2drive
```

```bash
chown -R nobody:nogroup /mnt/pve/backups && chown -R nobody:nogroup /mnt/pve/fujiinside && chown -R nobody:nogroup /mnt/pve/m2drive
```

```bash
chmod 777 /mnt/pve/backups && chmod 777 /mnt/pve/fujiinside && chmod 777 /mnt/pve/m2drive
```

Establish File Permissions

```bash
chmod 777 /mnt/backups && chmod 777 /mnt/fujiinside && chmod 777 /mnt/m2drive
```

Grant NFS Access
Mount permamently curently Linux TurnKey (FS-trnt)

```bash
nano /etc/exports
```

```bash
192.168.1.1:/backups /mnt/backups nfs  rw,async,auto,_netdev  0  0
192.168.1.1:/fujiinside /mnt/fujiinside nfs  rw,async,auto,_netdev  0  0
192.168.1.1:/m2drive  /mnt/m2drive  nfs  rw,async,auto,_netdev  0  0
```

restart the NFS Kernel Server

```bash
sudo systemctl restart nfs-kernel-server
```

Grant NFS Access through the Firewall (FS-trnt: not necesary)

```bash
sudo ufw allow from [clientIP or clientSubnetIP] to any port nfs
```

```bash
sudo ufw allow from 192.168.1.1 to any port nfs
```

if the firewall is turned off, enable it:

```bash
sudo ufw enable
sudo ufw status
```

Mount on Proxmox curently permamently

>NOTE: Best and what works now is mounting using prox UI. Select `Datacenter` then `Storage` >> `Add` and input values.
{: .prompt-warning }

```bash
nano /etc/fstab
```

```bash
192.168.1.1:/mnt/backups /mnt/backups nfs  rw,async,auto,_netdev  0  0
192.168.1.1:/mnt/fujiinside /mnt/fujiinside nfs  rw,async,auto,_netdev  0  0
192.168.1.1:/mnt/m2drive  /mnt/m2drive  nfs  rw,async,auto,_netdev  0  0
```

- Mount drive to container. Currently in `FS-trnt` container

```bash
nano /etc/pve/lxc/102.conf
```

```bash
mp0: SSD:subvol-102-disk-1,mp=/mnt/internalssd,size=700G
mp1: /mnt/fujiinside,mp=/mnt/fujiinside
mp2: /mnt/m2drive,mp=/mnt/m2drive
mp3: /mnt/backups,mp=/mnt/backups
```

Windows-Client

- correct registry [Details in here](https://graspingtech.com/mount-nfs-share-windows-10/)

1. Open regedit by typing it in the search box end pressing Enter
2. Browse to 'HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default'
3. Create a new New DWORD (32-bit) Value inside the Default folder named 'AnonymousUid' and assign the UID found on the UNIX directory as shared by the NFS system
4. Create a new New DWORD (32-bit") Value inside the Default folder named AnonymousGid and assign the GID found on the UNIX directory as shared by the NFS system.

- check UID and GID

```cmd
mount -o anon \\192.168.1.1\mnt\backups P:
mount
```

- mount a share on the NFS system at /mnt/vms

```cmd
mount \\<IP_ADDRESS>\<PATH_TO_DIR>\ drive:
mount \\<IP_ADDRESS>\mnt\backups Z:
```

- mount Currently in `FS-trnt` container

```cmd
mount \\192.168.1.1\\mnt\backups\ P:
mount \\192.168.1.1\mnt\backups\ P:
```

## Extras

- KILL stuck VM. get PID

```bash
ps aux | grep "/usr/bin/kvm -id VMID"
kill -9 PID
```

- in windows explorer type in

```bash
\\IP_ADDRESS\mnt\sdd-data
```

- [iGPU Full Passthrough](https://3os.org/infrastructure/proxmox/gpu-passthrough/igpu-passthrough-to-vm/#proxmox-configuration-for-igpu-full-passthrough)

```bash
nano /etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT="quiet"
# change to:
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
# for CPU

GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction initcall_blacklist=sysfb_init video=simplefb:off video=vesafb:off video=efifb:off video=vesa:off disable_vga=1 vfio_iommu_type1.allow_unsafe_interrupts=1 kvm.ignore_msrs=1 modprobe.blacklist=radeon,nouveau,nvidia,nvidiafb,nvidia-gpu,snd_hda_intel,snd_hda_codec_hdmi,i915"

nano /etc/modules

vifo
vfio_iommu_type1
vifo_pci
cifo_virqfd
```
