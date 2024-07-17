## Network Enumeration notes  

### Nmap  

Host discovery scan - The default host discovery done with -sn consists of an ICMP echo request, TCP SYN to port 443, TCP ACK to port 80, and an ICMP timestamp request by default
```  
sudo nmap -sn 192.168.42.0/24
```

To create a file with the Up Hosts:  
```
sudo nmap -sn 192.168.42.0/24 | awk '/Host is up/ {print a} {a=$0}' | awk '{print $NF}' > uphosts.txt
```

Load hosts from a list (-iL), scan all ports (-p-), Enable OS detection, version detection, script scanning, and traceroute (-A), and output in all file formates (-oA):  
```
sudo nmap -iL uphosts.txt -p- -A -oA scan_output
```

Convert Nmap XML output to CSV:
```
git clone https://github.com/laconicwolf/Nmap-Scan-to-CSV.git
python nmap_xml_parser.py -f scan_output.xml -csv scan_output.csv
```


### GoBuster  

To perform dirbusting:
```
gobuster dir -u http://192.168.42.42 -w /usr/share/wordlists/dirb/common.txt -o results_42.txt -x txt,pdf,config
```
