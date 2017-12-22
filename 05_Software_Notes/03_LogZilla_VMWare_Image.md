<!-- @@@title:LogZilla VMWare Image@@@ -->

LogZilla on VMWare
---

Users may <a href="http://www.mediafire.com/file/qshaa7rldep9g2o/LZ5.ova">download</a> a LogZilla instance for use in testing or smaller scale deployments.

For larger deployments, this VM may still be used, but your System Administrator will need to add a second disk (and likely more Ram and CPU) to the VM to ensure that all data can be stored and processed at scale.

The default disk size in the LogZilla VM is 50GB. Adding a second disk to the VM is quite simple as it is pre-configured to use Linux's Logical Volume Manager (LVM).

Adding More Disk Space
---
>Note: The VM does not need to be powered off in order to add more disk space.

To add more disk to the VM using VMWare, you must add a *new disk* which will be formatted in the OS after adding it. *Do not* attempt to grow the current VM Disk instead of just adding that second disk. After adding the second disk, connect to the console or SSH to the running Logzilla Server and use the following commands:

All commands should be run as root (`sudo su -`)
Type `fdisk -l | grep /dev/[sv]` and locate the name of the second disk recently added, for example:

    root@myhost [~]: # fdisk -l | grep /dev/[sv]
    Disk /dev/vda: 34.4 GB, 34359738368 bytes
    /dev/vda1   *        2048      499711      248832   83  Linux
    /dev/vda2          501758    67106815    33302529    5  Extended
    /dev/vda5          501760    67106815    33302528   8e  Linux LVM
    Disk /dev/vdb: 107.4 GB, 107374182400 bytes
    
In this case, the new disk is `/dev/vdb`.

Paste the following commands:

    disk="/dev/vdb"
    printf 'n\n\n\n\n\nt\n8e\np\nw\n' | fdisk -c -u $disk
    vg=$(lvdisplay | grep "VG Name" | head -1 | awk '{print $3}')
    part=1
    lvpath=$(lvdisplay | grep Path | grep root | awk '{print $3}')
    pvcreate ${disk}${part}
    vgextend ${vg}  ${disk}${part}
    lvextend -l+100%FREE ${lvpath}
    resize2fs ${lvpath}

After pasting, you should see a message stating that the root volume has been resized. Nothing more is needed.

If you do not have have VMWare Server or Workstation, you may <a href="https://www.vmware.com/products/player">download the VMWare player</a> for free from VMWare, Inc.
