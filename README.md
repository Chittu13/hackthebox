# Hackthebox
- [Nmap](#nmap)
- [BurpSuite](#burp)
- [Gobuster](#gobuster)

- ## Nmap <a name="nmap"></a>
- nmap -sC -sV -oA nmap/report.txt <ip>

- ## Burp Suite <a name="burp"></a>
- use ```/manager/html``` or ```/..;/manager/html``` in the middel of ```GET/ HTTP/1.1```
  - so it will like this ```GET/ /..;/manager/html HTTP/1.1```
  - whenever you see JSESSIONID=.... use this.
 
- ## Gobuster <a name="gobuster"></a>
- ```gobuster dir -u <url> -k -w /opt/SecLists/Discovery/Web-Content/raft-small-words.txt```
 
