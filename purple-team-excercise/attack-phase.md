# ATTACK PHASE
Attacking Metasploit2 VM to find/exploit vulnerabilities.
## Methodology
 1. Discover services and versions w/ nmap (Network Enumeration).
 2. Look up the service versions to discover potential exploits/vulnerable versions.
 3. Display PoC (Proof of Concept) by manually exploiting.


# 1. Recon/Fingerprinting
## Command 1 (TCP scan w/ vulnerability script): sudo nmap -p- -sV -sC -A [IP_ADDR]


<img width="618" height="653" alt="image" src="https://github.com/user-attachments/assets/e1128637-cae0-447f-9989-0ed95f16df12" />

OS discovery - Linux 2.6.9 - 2.6.33
Hosts: metasploitable.localdomain, irc.Metasploitable.LAN
Services found:
1. Port 21 - FTP | version: vsdtpd 2.3.4
   - File transfer protocol
   - This specific version contains a malicious backdoor (CVE-2011-2523)
   - Allows unauthenticated users to execute arbitrary commands w/ root privileges.
   - If user logins in with any username ending with ":)" and any password, they get root access on the system.
   - Use netcat to connect to port 21 and sign in.
   - Spawn a different instance of netcat to access backdoor on port 6200
   - Exploit successful - root access gained
   <img width="1568" height="418" alt="image" src="https://github.com/user-attachments/assets/de8d666e-5b4a-4862-bc72-ac60ee14027d" />

2. Port 22 - SSH | version: OpenSSH 4.71
   - Known outdated/vulnerable version of SSH
   - CVE-2018-15473
   - There is a python script available on github to enumerate usernames
     <img width="1250" height="864" alt="image" src="https://github.com/user-attachments/assets/412c1693-c2ae-4289-b8bf-c12072db2ba8" />

3. Port 23 - Telnet | version: Linux telnetd
   - Outdated service
   - Known exploit: CVE-2026-24061
   - Successful, telnet access gained.
   - This works because of weak input validation, '-f' flag stands for force. So '-f root' force logins as root.
   <img width="1124" height="780" alt="image" src="https://github.com/user-attachments/assets/5f124a90-0dcc-4944-9fc3-27666872001a" />

4. Port 25 - SMTP | version: Postfix smptd
   - This version has a known exploit: CVE-2023-51764
     
5. Port 53 - Domain | version: ISC BIND 9.4.2
   - Version 9.4 has known exploits and has been EoL since 2012. (No longer receives updates)
   - 
6. Port 80 - HTTP | version: Apache httpd 2.2.8 ((Ubuntu DAV/2)
7. Port 111 - Rcpbind | version: 2 (RPC #100000)
8. Port 139 - Netbios-ssn | version: Samba smbd 3.X - 4.X
9. Port 445 - Netbios-ssn | version: Samba smbd 3.0.20-Debian
   - Vulnerable to CVE-2007-2447
   - Remote Command Execution Attack
   - Exploit available in msfconsole 'exploit/samba/username_script'
    <img width="1272" height="406" alt="image" src="https://github.com/user-attachments/assets/6c572a88-ad0a-4e81-abe9-a4290de93914" />
    <img width="1136" height="1272" alt="image" src="https://github.com/user-attachments/assets/6d315306-3355-4ca9-bccc-ca0803a4c464" />


10. Port 512 - Exec | version: netkit-rsh rexecd
11. Port 2049 - NFS | version: 2-4 (RPC #100003)
12. Port 2121 - FTP | version: ProFTPD 1.3.1
13. Port 3306 - MySQL | version: MySQL 5.0.51a-3ubuntu5
   - Weak authentication exploit
     <img width="1240" height="422" alt="image" src="https://github.com/user-attachments/assets/d94e5c71-1b69-4514-bbe4-5463745df0dd" />

14. Port 3632 - Distccd | versiion: Distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
15. Port 5432 - PostgreSQL | version: PostgreSQL DB 8.3.0 - 8.3.7
16. Port 5900 - VNC | version: VNC (protocol 3.3)
17. Port 6667 - IRC | version: UnrealIRCd
18. Port 8009 - AJP13 | version: Apache Jserv (Protocol v1.3)
19. Port 8180 - HTTP | version: Apache Tomcat/Coyote JSP engine 1.1
20. Port 8787 - DRB | version: Runy DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
21. Port 39402 - Mountd | version: 1-3 (RPC #100005)
22. Port 56818 - Nlockmgr | version: 1-4 (RPC #100021)
23. Port 60588 - Status | version: 1 (RPC #100024)

## Command 2 (UDP scan): sudo nmap -sU -sV --top-ports 200 --min-rate 1000 [IP_ADDR]



## Command 3:


All found services:
