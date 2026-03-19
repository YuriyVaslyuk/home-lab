## Home Lab Setup
![Lab Setup](lab-setup.jpg)

### Network Diagram

SaskTel Router (Home network - 192.X.X.X) -->

-->  Linksys E4200 (Lab gateway - 192.X.X.X) (Bought a used router for the lab) -->

  --> D-Link DES-1024D 24-Port unmanaged switch (Got it for free, I wiil get a managed one as soon as I can) -->
      --> Windows 7 Target machine (Older Lenovo ThinkCentre)

### Lab Router - Linksys E4200
- IP: Set up ....
- DHCP range: Set up ....
-  Security: WPA2 Personal
-  Remote Management: Disabled
-  UPnP: Disabled
-  Admin password: set

### Windows 7 Target Machine
- Lenovo ThinkCentre
- Intel i5-4570 @ 3.20GHz
- 4GB RAM
- 500GB HDD (replaced failing drive)
- Windows 7 Professional
- IP: X.X.X.X (DHCP)
- Purpose: vulnerable legacy target for exploitation practice

### Troubleshooting Notes
- Original hard drive failed - clicking sounds + CHKDSK abort
- Replaced with spare 500GB HDD, clean install worked
- After changing router to a custom lab subnet separate from home network
  the target machine lost connection
- Fixed with: ipconfig /release then ipconfig /renew
- This forces Windows to request a new IP from the router
  on the updated network range

### Planned Additions
- I currently have a few more older towers that could be added to the lab.
- Tower 2: Windows 10/11 target for a newer system practice
- Tower 3: Ubuntu target  for Linux practice
- Lexmark laser printer: IoT target device (Plus additional hardware troubleshooting skill)
- TP-Link managed switch: for VLAN practice (Unless I find a different used one)
- Raspberry Pi: DNS server or monitoring tool
- Kali Linux VM on laptop: primary attack machine (Using VM on my personal laptop since it is the most powerful one out all the units.

## Lab Sessions

### March 18, 2026  (EternalBlue on Physical Hardware)

Got Kali 2025.4 running on VirtualBox tonight and connected it to the lab network as a bridged adapter. Kali picked up 192.168.10.126 on the same subnet as LAB-WIN7.
I am using my personal daily laptop, which is why I went with VirtualBox; it seems to run quite well.

Ran Nmap from Kali against LAB-WIN7 (192.168.XX.XX) and saw port 445 open. Fired up Metasploit, used ms17_010_eternalblue, set RHOST and LHOST and ran it. Got a Meterpreter shell back fast. (I was surprised how fast Nmap worked on the virtual machine. Even though I did this not that long ago through THM, geting this done on a real hardware was very fun.

Ran getuid and got NT AUTHORITY\SYSTEM  - full control of the machine. Executed calc.exe remotely and watched it pop up on the physical screen across the room. Same exploit I did on TryHackMe Blue room. These tangible results make me want to explore more, and also make me want to learn defensive security.

Attack machine: Kali 2025.4 VM — 192.168.XX.XX
Target: LAB-WIN7 Windows 7 SP1 — 192.168.XX.XX The IP addresses are all sequential, as I only have 3 devices on it at the moment of writing this.
Exploit: ms17_010_eternalblue
Result: Meterpreter shell, NT AUTHORITY\SYSTEM
