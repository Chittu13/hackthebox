- __```cat /etc/os-release```__
- __```cat /etc/passwd| column -t -s :``` or ```cat /etc/passwd```__
- __```cat /etc/group```__

### 1. Login information
- __In the `/var/log` directory, we can find log files of all kinds including `wtmp` and `btmp`. The `btmp` file saves information about failed logins, while the `wtmp` keeps historical data of logins.__
- __`sudo last -f /var/log/wtmp`__
  - > __These files are not regular text files that can be read using `cat`, `less` or `vim`; instead, they are binary files, which have to be read using the `last` utility__

### 2. Authentication logs
- __`/var/log/auth.log`__

### 3. Network Configuration
- __To find information about the network interfaces, we can `cat` the `/etc/network/interfaces` file__


### 4. Active network connections
- __On a live system, knowing the active network connections provides additional context to the investigation. We can use the `netstat` utility to find active network connections on a Linux host.__
  - `__netstat -natp`__
