After creating a Pfsense VM in Proxmox, I found that no link up bridge was connected and a second network interface seperate is required.
To fix this, I visited my Proxmox VE -> Hardware -> Add a network device.

<img width="599" height="187" alt="image" src="https://github.com/user-attachments/assets/df81abde-97a9-4afd-a6c7-c2c615c90722" />




These are the VLAN assignments.
<img width="1281" height="480" alt="image" src="https://github.com/user-attachments/assets/c748779e-4f3a-4c73-a5e2-a5d880618f03" />

1. MANAGEMENT (10)
2. TRUSTED (20)
3. LAB (99)

VLAN assignments
<img width="1270" height="388" alt="image" src="https://github.com/user-attachments/assets/47fb0771-019c-4206-adac-ba13da62226d" />



In order for DHCP server to work, I learned that the subnets must be large enough for the range of IP addresses. LAN, OPT2, and OPT3 subnets all set to /24.





<img width="1221" height="318" alt="image" src="https://github.com/user-attachments/assets/5f9730a7-ce69-4de0-9ba4-e9ffd11fd6eb" />
