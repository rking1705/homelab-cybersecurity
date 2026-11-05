# ATTACK PHASE
Attacking Metasploit2 VM to find/exploit vulnerabilities.
1. Recon/Fingerprinting
## Command 1 (TCP scan w/ vulnerability script): sudo nmap -p- -sV --script vuln -T4 [IP_ADDR]


<img width="618" height="653" alt="image" src="https://github.com/user-attachments/assets/e1128637-cae0-447f-9989-0ed95f16df12" />



## Command 2 (UDP scan): sudo nmap -sU -sV --top-ports 200 --min-rate 1000 [IP_ADDR]



## Command 3:


All found services:
