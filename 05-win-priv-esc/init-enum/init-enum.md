## Initial Enumeration

After getting a foothold on a windows machine I'll typically run a couple things manually to start:

> whoami

To see my priviledges:

> whoami /priv

To see my groups

> whoami /groups

See users on this machine

> net user

System Info
```
systeminfo
hostname
```

### Network

> ipconfig /all

check the arp table aand see who the server is talking to

> arp -a

Check out the route table 

> route print

What ports are open

> netstat -ao

### Password Hunting

Try running findstr command for passswords
> findstr /si password *.txt *.ini *.config

### AV Enumeration

See all running services and look for av services
> sc queryex type= service

Can check for service information on any running av service
> sc query windefend



