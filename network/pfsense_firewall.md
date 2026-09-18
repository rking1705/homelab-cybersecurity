#pfSense Setup

## Purpose
pfSense serves as the virtual firewall and network segmentation layer for the home lab environment.
Running as a VM on Proxmox, it isolates offensive lab traffic from trusted devices and management infrastructure.
This prevents a compromised lab VM from affecting the rest of the network.



I chose to use pfSense because it is open source and has come up while studying for the Comptia CySA+ exam.

pfSense requires two virtual NICs in Proxmox:
  - em0 - WAN interface, connects to home network (192.168.1.X)
  - vtnet0 - LAN trunk, carries all VLAN traffic
  

After creating a pfSense VM in Proxmox, the interface assignment wizard failed repeatedly with an error message 'no link detected'.
The VM only had one network card while pfSense requires at least 2. To fix this I added a second virtual NIC (vtnet0) through Proxmox:
VM -> Hardware -> Add -> Network Device

<img width="599" height="187" alt="image" src="https://github.com/user-attachments/assets/df81abde-97a9-4afd-a6c7-c2c615c90722" />

Upon reboot, the setup wizard completed successfully.



These are the VLAN assignments.
<img width="1281" height="480" alt="image" src="https://github.com/user-attachments/assets/c748779e-4f3a-4c73-a5e2-a5d880618f03" />

1. MANAGEMENT (10) |LAN| : 10.10.10.100 - 10.10.10.200 --> Management systems (Proxmox, pfSense)
2. TRUSTED (20) |OPT2| : 10.20.20.100 - 10.20.20.200 --> Trusted, daily devices
3. LAB (99) |OPT3| : 10.99.99.100 - 10.99.99.200 --> Lab (Kali, attack VMs)


VLAN assignments
<img width="1270" height="388" alt="image" src="https://github.com/user-attachments/assets/47fb0771-019c-4206-adac-ba13da62226d" />

After configuring VLANs, the pfSense web UI timed out and became unreachable because the laptop I am managing it from is on 192.168.1.X.
To fix this I went into the pfSense VM terminal and entered 'pfctl -d' to temporarily disable the firewall so I could add a WAN firewall rule allowing HTTPS access the the WAN address.
Upon reboot all firewall rules were active, while I maintained access to the web UI.

Firewall rules w/ descriptions:
<img width="1221" height="318" alt="image" src="https://github.com/user-attachments/assets/5f9730a7-ce69-4de0-9ba4-e9ffd11fd6eb" />



## Lessons Learned
  - In order for DHCP server to work, I learned that the subnets must be large enough for the range of IP addresses. LAN, OPT2, and OPT3 subnets are now all set to /24.
  - In a proper setup, the management device would already sit on the management VLAN (10.10.10.X) to access the web UI through LAN rather than WAN. I ran into the issue with losing access to the web UI because all of the physical devices in my setup are on the home network.
  - I learned that 2 NICs are required for pfSense (WAN and LAN), and how to add a new virtual NIC.
  - Implementing rules that protect devices, while still providing functionality
  - Wazuh and future management tools need to be moved to the management VLAN 







