# Hackthebox
- [Nmap](#nmap)
- [BurpSuite](#burp)
- [Gobuster](#gobuster)
- [FFUF](#ffuf)

- ## Nmap <a name="nmap"></a>
- nmap -sC -sV -oA nmap/report.txt <ip>

- ## Burp Suite <a name="burp"></a>
- use ```/manager/html``` or ```/..;/manager/html``` in the middel of ```GET/ HTTP/1.1```
  - so it will like this ```GET/ /..;/manager/html HTTP/1.1```
  - whenever you see JSESSIONID=.... use this.
 
- ## Gobuster <a name="gobuster"></a>
- ```gobuster dir -u <url> -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt```

- ## FFUF <a name="ffuf"></a>
- FFUF is used to filter the invalid pages 
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt```
- ```ffuf -u <url>/FUZZ -k -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -r -fs 27200```
  - if you are getting the size same of all use filter size and give the size there.
