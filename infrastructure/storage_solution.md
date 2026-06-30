# Storage Solution For Homelab

Problem: Laptop for the lab only has 256GB of storage which is not ideal for the virtual environments I want to set up.
I will be converting my old gaming desktop into a TrueNAS server which will keep my laptop lightweight and fast.

Laptop: AMD Ryzen 5 6600H, 24GB RAM, 256GB NVMe SSD
Desktop: Intel i5-9600K, 16GB RAM, 2TB HDD

1. Flash TrueNAS ISO onto flashdrive using Etcher (https://etcher.balena.io/#download-etcher)
2. Boot desktop from flashdrive/Install TrueNAS
3. Visit web user interface through given IP address. ex. (http://192.168.X.X) 
4. Create a pool
  - At this step, I got stuck when I went to create a pool and saw no drives were available
  - After troubleshooting, I found that TrueNAS servers requires a dedicated drive for the OS seperate from the storage pool.
  - Optimally, you would have multiple drives with RAIDZ for redundancy. 
  - However, a USB works aslong as you are aware this creates a higher potential for data loss
  - Data loss can occur here because since we are only using 1 USB drive, the only option is striping which means no redundancy.
5. Create a dataset
6. Now to make the folder accessible to the laptop running linux, we create a UNIX (NFS) share.
  - This creates a file path that will be made accessible to the laptop
Storage solution is setup.


I have additionally connected my VMs to my trueNAS server. They run local on the proxmox server to optimize speed, but use the NAS server as additional storage


*Issues
  TrueNAS server was powered off due to an outage. After powering back on, the web interface remained offline and the IP address was not responding to pings. After connecting a monitor to the server,
  I found that there was an issue during boot and it presented a blank screen w a blinking cursor.
