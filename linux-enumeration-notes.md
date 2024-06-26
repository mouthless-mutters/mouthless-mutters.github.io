## Linux Enumeration Notes 

### Information Gathering  
Name of the host  
```
hostname
```

Other system information  
```
cat /etc/issue
```
```  
cat /etc/os-release
```
```  
uname -a
```  

Current user and their group memberships  
```
id
```  

All users  
```
cat /etc/passwd
```

Get running processes with a and x flags to list all processes with or without a tty7 and the u flag to list the processes in a user-readable format  
```
ps aux
```

Network information  
```
ipconfig
```
```  
ip a
```
```  
route
```
```  
route1
```
```  
netstat
```
```  
ss -anp
```  

IPtable rules  
```
cat /etc/iptables/rules.v4
```

Cron jobs  
```
ls -lah /etc/cron*
```
```
grep "CRON" /var/log/syslog
```

Installed applications  
```
dpkg -l
```  

Find writable directories  
```
find / -writable -type d 2>/dev/null
```

List mounted filesystems  
```
cat /etc/fstab
```
```
mount
```

View available disks
```
lsblk
```

Find SUID binaries  
```
find / -perm -u=s -type f 2>/dev/null
```
