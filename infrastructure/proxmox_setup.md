# Proxmox Setup

## Hardware
Flashed Proxmox OS onto Lenovo Ideapad gaming Laptop
  - AMD Ryzen 5 6600H, 12 logical cores
  - 24GB RAM
  - 256GB NVMe SSD

I decided to flash my laptop with Proxmox VE due to the simplicity of running a bare-metal hypervisor directly on the hardware, rather than running it ontop of an OS.
This allows for better performance and resource allocation while running multiple VMs at once.

1. Downloaded Proxmox VE 9.2 ISO from proxmox.com
2. Flashed to USB using Balena Etcher
3. Boot from stick, Install Proxmox VE
4. Configure root credentials


## Issue 
After installation, the console displayed the URL to visit the UI. However, the web UI was unreachable from all devices on the same network.
To troubleshoot, I ran "ip addr show" to reveal that both physical interfaces (ethernet 'nic0' and Wifi "wlo1") were both in a DOWN state.
Only the virtual bridge 'vmbr0' had an IP address, but had no physical interface attached as a bridge port.
*This explains why we were able to see a valid web UI URL without access.
## Fix
Checking '/etc/network/interfaces' confirmed that the bridge was configured to use 'nic2' (USB ethernet adapter) instead of 'nic0' (Laptop built-in ethernet):


<img width="952" height="1269" alt="network-bridging" src="https://github.com/user-attachments/assets/bd8b4a61-74c6-4516-9b1b-bc1c16c92943" />


Using commands:
"sed -i 's/bridge-ports nic2/bridge-ports nic0/' /etc/network/interfaces"
followed by:
"ifreload -a"
Solved the issue and finished configuration through the web UI.



## VM Storage Strategy
I initially mounted TrueNAS as NFS storage for all VM disks to maximize capacity. However, this created a dependency. Anytime TrueNAS would power off (power outage, downtime, reboot) made the VMs unusable.
Learned that VM disks for critical/active VMs (Ubuntu for Wazuh, pfSense) should be stored locally on local-lvm (NVMe) for reliability. TrueNAS is still listed as a storage for proxmox, but now will only store log files, ISO files, and backups.

## Mounting TrueNAS as storage (NFS)
Through web UI as admin:
  Datacenter -> Storage -> Add -> NFS
    - ID: truenas
    - Server: (TrueNAS IP Address) 
    - Export: (Path to dataset created in TrueNAS pool)
    - Content: Disk Image, ISO image, VZDump backup, Snippets



## Issue
Error message displayed when remounting TrueNAS as Proxmox storage after power outage required reconfiguring the NFS share*
create storage failed: failed to create content directory 
'/mnt/pve/truenas/images' - Permission denied

Required visiting TrueNAS web UI to edit dataset permissions. Default datasets block root from read/write/execute access and require setting Maproot User/Group to root/wheel on the NFS share to create storage on proxmox.


## VMs Currently Running on Proxmox
  - pfSense
  - Ubuntu (Wazuh)
  - Kali Linux

