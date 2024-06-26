## Port Forwarding

### Socat examples  
```
socat TCP-LISTEN:<LPORT>,fork TCP:<RHOST_IP>:<RPORT>
```  

Verbosely (-ddd) listen on port 2345 and forward traffic to remote host 10.10.10.42 on port 5432
```
socat -ddd TCP-LISTEN:2345,fork TCP:10.10.10.42:5432
```
