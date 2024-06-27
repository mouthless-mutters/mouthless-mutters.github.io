## Port Forwarding

### Socat examples  
```
socat TCP-LISTEN:<LPORT>,fork TCP:<RHOST_IP>:<RPORT>
```  

Verbosely (-ddd) listen on port 2345 and forward traffic to remote host 10.10.10.42 on port 5432
```
socat -ddd TCP-LISTEN:2345,fork TCP:10.10.10.42:5432
```

### SSH local port forwarding
```
ssh <local_ip>:<local_port>:<remote_ip>:<remote_port> <username>@<ssh_server>
```

Creates a SSH connection on 10.10.10.42 and sets up local port forarding (-L) on port 2222, using -N to prevent SSH from executing remote commands. Any traffic sent through port 2222 on 10.10.10.42 will be sent to port 22 on 172.16.10.42
```  
ssh -N -L 0.0.0.0:2222:172.16.10.42:22 admin@10.10.10.42
```

### SSH dynamic port forwarding
```
ssh -N -D <local_ip>:<local_port> <username>@<ssh_server>
```

Creates a SSH connection on 10.10.10.42 and sets up dynamic port forarding (-D) on port 9999, using -N to prevent SSH from executing remote commands. Any traffic sent through port 9999 on 10.10.10.42 will be sent to the specified host and port.
```  
ssh -N -D 0.0.0.0:9999 admin@10.10.10.42
```

### SSH Remote port forwarding

Creates a connection from a remote host to local SSH server (192.168.10.42), and forwards the local port 2345 to a remote host 10.10.10.42 on port 5432. 
```
ssh -N -R 127.0.0.1:2345:10.10.10.42:5432 local_admin@192.168.10.42
```

### SSH Remote dynamic port forwarding

Creates a connection from a remote host to a local SSH server (192.168.10.42), where any traffic going through port 9999 on that host will route to the appropirate host and port.
```
ssh -N -R 9999 local_admin@192.168.10.42
```

### Shuttle

``` 
sshuttle -r <username:password@<ssh_addr>:<port> <subnet_to_route> <subnet_to_route>
```

Creates SSH remote connection to 192.168.1.42 and routes any traffic to the specific subnets through the connection.
```
sshuttle -r admin@192.168.1.42:2222 10.10.10.0/24 172.16.10.0/24
```

### Use with proxychains
Edit /etc/proxychains4.conf to add socks5 10.10.10.42 9999  
nmap example:  
```
proxychains nmap -Pn 172.16.10.42
```

Note, lowering the tcp_read_time_out and tcp_connect_time_out values in /etc/proxychains4.conf dramatically decrease the scan times.

