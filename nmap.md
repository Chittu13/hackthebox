# Nmap

# Nmap using ping scan only
- __`nmap -sn 10.5.16.13` -------> only ping scan it won't scan for ports__
- __`nmap -sn <ip>/24` ------> for subnet ARP scan only__
- __`nmap -sn <ip>/24`  --send-ip  ------> for subnet ICMP scan, TCP SYN scan, ARP scan__
- __`nmap -sn <ip>-240` -------> scanning how many host are alive in subnet using nmap__
- __`nmap -sn -iL <list_of_IPs_file>`  --------> it will check, how many IPs are live that you gave in the IPs file__
- __`sudo nmap -sn <ip>/24`__
- __`netdiscover -I eth0 -r <ip>/24`__

# Nmap using port scan only 
- __-Pn -----> No host Discovery or no ping__
- __-PS -----> 2 way handshak in host Discovery__
- __-PE -----> imple ICMP echo packet__
- __`ping -c 5 <ip>`__
  - __-c -----> number of packets__
- __`ping -n 5 <ip>`   -------> command for windows__
# ICMP echo request
- __`fping -a -g <ip>/24 2>/dev/null` -----> for the subnet__
- __`fping -a <ip>`  -------> for single ip__

## TCP SYN scan(it send only SYN pack to the target)
- __`nmap -sn -PS1-1000 <ip>`__
  + __-PS -----> it will send only syn pack only two way hand shake__
  + __-PS80 or -PS80,22,21 or -PS1-1000 ----> it will scan specify port that host is live or not by default port 80 it will scan__

## ICMP Scan 
- __`nmap -sn -PE <ip>` ----> simple ping scan ICMP echo packet__






## Port Scanning With Nmap
- __`nmap -Pn -sU <ip>`      ---> UDP Scan__
- __`nmap -Pn -F -sV -O <ip> -v`__
- __-F  -----> First 100 popular ports it will scan__
- __-sV -----> Version Detection__
- __-O  -----> OS Detection__
- __-sC -----> Nmap script__ 
- __-v  -----> verbous__
- __-F  -----> 100 popular port scan__
- __-sT -----> 3 way handshak in port scanning__
- __-sS -----> 2 way handshak in port scanning__
- __-A  -----> Agressive scan combing -sV(Version Detection), -O(OS Detection), -sC(Default Script Scan)__
## -T<0-4>
  - __-T0 ------> paranoid scan__
  - __-T1 ------> sneaky scan__
  - __-T2 ------> polite scan__
  - __-T3 ------> normal scan__
  - __-T4 ------> aggressive scan__
  - __-T5 ------> insane scan__


## Nmap Scripting Engine
__`ls -al /usr/share/nmap/scripts/`__

__`nmap --script-hlep=<script_name>`__
__for example:__
__`nmap --script-help=mongodb-databases`__
  -  __It will show the info about that script__

## To Run particular script 
- __`nmap -sS -sV --script=mongodb-databases -p- __-T4 <ip>`__

## Optimizing Nmap Scans

- __`nmap -sS -sV -F --host-timeout 30s 10.0.23.1/24`__
- __`nmap -sS -sV -F --scan-delay 15s 10.0.2.5`__
- __`nmap -sS -sV -F -T1 10.0.2.5`__
### To make the nmap scan comming from the different ip address 
+ __-D option is used for decoy scanning. This technique involves sending scan packets from multiple IP addresses__
+ __`nmap -D RND:10 <target IP>`__
+ __`RND:10` tells Nmap to use 10 random decoys in addition to your real IP address.__
- __For example:__
__`nmap -Pn -sS -sV -p 80,443 -f --data-length 200 -D <gate_way_ip>` remove last digit form you ip address add 1 at the last so it's become router address__
 + __`nmap -Pn -sS -p 80,443 -f --data-lenght 200 -D 10.0.2.1, 10.0.2.2`__
### To make the nmap scan comming from the certain port
__`nmap -g 53 <target IP>`__
+ __Nmap will send packets with a source port of 53__
+ __so it apperse as request is comming from the DNS server because 53 port is a DNS server__



# Enumeration
- ## Nmap <a name="nmap"></a>
- ```nmap -sC -sV -vv -oA nmap/report.txt <ip>```
- ```sudo nmap -Pn -A -p- -T4 <ip> -o nmap.txt```
- [Squid](https://github.com/aancw/spose.git) Pivoting Open Port Scanner
  - Detecting open port behind squid proxy for CTF and pentest purpose using http proxy method. Only for Python 3 version.
  - ```python3 spose.py --proxy http://192.168.208.189:3128 --target 192.168.208.189```
  - if you detect new port for example: 8080 then you need to config your proxy in firefox set ```HTTP proxy 192.168.208.189``` and ```port 3128```
  - then only you can viste the website http://192.168.208.189:8080
