# Pi-hole Setup

## Hardware
- Raspberry Pi 4 Model B (1GB RAM)
- Running Raspberry Pi OS Lite
- Connected via WiFi 

## Purpose
Pi-hole serves as the network-level DNS filtering and monitoring system.
It has the capability to block ads, track requests, and comparing domains against a large malware domain blacklist (Steven Black's Unified Hosts List.
This adds another layer of protection for all lab devices.

## Installation
"curl -sSL https://install.pi-hole.net | bash"

Rather than configuring DNS on each device, I was able to set a DNS server in pfSense so that all requests made by my lab devices are passed through Pi-hole.

pfSense: Services --> DHCP Server --> set DNS server on each VLAN to the IP address of the raspberry pi.

Screenshot of Pi-hole blocking requests made by lab devices:
<img width="2516" height="1014" alt="rasppi" src="https://github.com/user-attachments/assets/feb5d0f0-6186-4591-90eb-e8848da91fd2" />
