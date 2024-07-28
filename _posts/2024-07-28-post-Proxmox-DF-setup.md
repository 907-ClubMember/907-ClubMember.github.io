---
title: "Setting up digital forensics on Kali hosted on Proxmox"
date: 2024-07-28T11:07:30-04:00
categories:
  - Setup
tags:
  - Homelab
  - Forensics
  - Kali
  - Proxmox
  - Setup
---
**Notice:** Do not do this a drive that will need chain of custody! This is very fast and loose and there are several parts where systems could modify the drive!
{: .notice--danger}

[Follow this guide for getting VM setup](https://forum.proxmox.com/threads/proxmox-beginner-tutorial-how-to-set-up-your-first-virtual-machine-on-a-secondary-hard-disk.59559)


### Passing through drive that will be investigated

#### Source [Passthrough Physical Disk to Virtual Machine Proxmox](https://pve.proxmox.com/wiki/Passthrough_Physical_Disk_to_Virtual_Machine_(VM))

#### First open up a terminal on he host and install lshw

``` 
apt install lshw
```

#### List all drives on the host
``` 
lshw -class disk -class storage
```

#### This will give the results such as this

```
*-disk
                description: ATA Disk
                product: ST3000DM001-1CH1
                vendor: Seagate
                physical id: 0.0.0
                bus info: scsi@3:0.0.0
                logical name: /dev/sda
                version: CC27
                serial: Z1F41BLC
                size: 2794GiB (3TB)
                configuration: ansiversion=5 sectorsize=4096
```

#### Run this command to get more information on the disk

``` 
ls -l /dev/disk/by-id | grep Z1F41BLC
ls -l /dev/disk/by-id | grep {serial number from your disk} 
```

#### You'll then get an output along the lines of 

``` 
lrwxrwxrwx 1 root root 9 Jan 21 10:10 /dev/disk/by-id/ata-ST3000DM001-1CH166_Z1F41BLC -> ../../sda

lrwxrwxrwx 1 root root 9 Jan 21 10:10 /dev/disk/by-id/ata-{your-drive's info} -> ../../sda
```

#### Adding drive

```
qm set 592 -scsi2 /dev/disk/by-id/ata-ST3000DM001-1CH166_Z1F41BLC
update VM 592: -scsi2 /dev/disk/by-id/ata-ST3000DM001-1CH166_Z1F41BLC

qm set {VMID} -scsi2 /dev/disk/by-id/ata-{your drive's info}
update VM {VMID}: -scsi2 /dev/disk/by-id/ata-{your drive's info}
```

#### Then stop and restart the VM to make sure it shows up


# Performing forensics  

#### Once you have Kali and the drive you want to investigate setup run Autopsy. By default it will run on localhost

![](/assets/images/autopsy_start.png)


### To clone the drive you have to clone [per partition and not the whole drive.](https://security.stackexchange.com/questions/161882/ntfs-volume-not-recognized-by-autopsy) DD the [partitions](https://www.cyberciti.biz/faq/unix-linux-dd-create-make-disk-image-commands/). A GUI example would be below of where to get the file partition to clone 

![](/assets/images/Kali_disk_GUI.png)

### Perform the following [To set up the case](https://wiki.sleuthkit.org/index.php?title=Autopsy:_Setting_Up_a_Case)

### Please refer to the post [Performing digital forensics on Kali]({% post_url 2024-07-28-post-Kali-DF %}) for performing forensics.