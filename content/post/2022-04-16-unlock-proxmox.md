---
title: How To Unlock a Proxmox VM
title: How To Unlock a Proxmox VM
subtitle: Startup Your Terminal
date: 2021-04-16
tags: ["proxmox", "vm"]
draft: true
---

![Locked Proxmox VM](/proxmox_lock.png)
 
 
| PROBLEM | If while attempting to start, stop or shutdown a virtual machine via the Proxmox GUI and the "Error: VM is locked" is displayed then follow this guide. |
|-|-|

 

-----
 
### Access the Terminal
First access Proxmox via the terminal. This can be done either by ssh

    ssh root@192.168.1.10

**or**

by clicking on your Node and then navigating to the ">_ Shell".

![Node](/proxmox_node_shell.png)
 
### Retrieve List of VM Configuration Files
**VMID** - Every Virtual Machine (VM) has a unique identifying ID number starting at 100 which is located before the name of the VM.  You may have to click on your Node in the resource tree to expand the list.

- **Simple Method**

        qm list

    ![qm list](/qmlist.png)
 
- **Advanced Methods**

        cat /etc/pve/.vmlist

    ![VM List](/vmlist1.png)

    **or**

1.      ls /var/lock/qemu-server

    ![VM List](/vmlist2.png)
 
2.      cat /etc/pve/qemu-server/<VMID>.conf

    ![VM List](/vmcat.png)
 
### Unlock Virtual Machine

    qm unlock <VMID>

- There is a possibility that you will receive an an error.
![VM Unlock Error](/vmunlocktimeout.png)
 
### Manually Delete Locked Configuration File
In the case you receive an error while attempting to unlock a VM then the configuration file may have to be deleted.

    rm  /var/lock/qemu-server/lock-<VMID>.conf

Run the unlock command again.

    qm unlock <VMID>
 
### Stopping the VM
If for some reason you are able to successfully unlock the VM but are still unable to shut it down or run some other process on it then you can attempt to force stop it
>>>**Follow the directions below at your own discretion and understand that running `qm stop` is like unplugging a computer and can cause data loss.**

    qm stop <VMID>
 
### Kill the VM Process
IF ALL ELSE FAILS! - PROCEED WITH CAUTION
**PID** - Process ID is a unique identifier for a particular process in Linux. 

    ps aux | grep "/usr/bin/kvm -id VMID"
    kill -9 <PID>

![VM Kill](/vmkill.png)
 
 
 
#### Contact 📧 <info@blackcursive.org>