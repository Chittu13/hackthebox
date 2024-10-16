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

### hash
```
hashcat -m 3200 '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' /usr/share/wordlists/rockyou.txt
```


- __You need to filter the below word list__
 -  `/usr/share/wordlists/SecLists/Fuzzing/1-4_all_letters_a-z.txt`
 -  __To get the words containg 4 characters__
  - __grep -E ‘^[a-zA-Z]{4}$’ /usr/share/wordlists/SecLists/Fuzzing/1-4_all_letters_a-z.txt > filteredwords.txt__
  - `hashcat -m 3200 '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' filteredwords.txt`
