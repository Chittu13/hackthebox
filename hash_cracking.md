- __[Cyber-Chef](https://gchq.github.io/CyberChef/)__
- __[Hash_Finder](https://www.tunnelsup.com/hash-analyzer/)__
- __[Decode_hashs](https://10015.io/tools/md5-encrypt-decrypt) like sha,md5...__
  - __[CrackStation](https://crackstation.net/) Alternative online tool for hack crakcing ntlm,md5,md4,sha__

### John The Ripper
```
john --format=descrypt --wordlist /usr/share/wordlists/rockyou.txt hash.txt
```

- __`hashid-m 'hash'`__

- **`hash-identifier '<hash_in_quotes>'`**

### MD5
```
hashcat -m 0 '48bb6e862e54f2a795ffc4e541caed4d' /usr/share/wordlists/rockyou.txt
```

### SHA1
```
hashcat -m 0 'CBFDAC6008F9CAB4083784CBD1874F76618D2A97' /usr/share/wordlists/rockyou.txt
```

### SHA256
``` 
hashcat -m 1400 '1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032' /usr/share/wordlists/rockyou.txt
```

### Hash SHA512
```
hashcat -m 1800 '$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.' /usr/share/wordlists/rockyou.txt
```

### NTLM
```
hashcat -m 1000 '1DFECA0C002AE40B8619ECF94819CC1B' /usr/share/wordlists/rockyou.txt
```

### HMAC-SHA1 (key = $salt)
```
e5d8870e5bdd26602cab8dbe07a942c8669e56d6 
Salt: tryhackme
```
  - `hashcat -m 160 'e5d8870e5bdd26602cab8dbe07a942c8669e56d6' /usr/share/wordlists/rockyou.txt`

### hash
```
hashcat -m 3200 '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' /usr/share/wordlists/rockyou.txt
```

- __You need to filter the below word list__
 -  __`/usr/share/wordlists/SecLists/Fuzzing/1-4_all_letters_a-z.txt`__
 -  __To get the words containg 4 characters__
  - __grep -E ‘^[a-zA-Z]{4}$’ /usr/share/wordlists/SecLists/Fuzzing/1-4_all_letters_a-z.txt > filteredwords.txt__
  - __`hashcat -m 3200 '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' filteredwords.txt`__
