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


### Accounts
Windows systems mainly have two kinds of users. Depending on their access levels, we can categorise a user in one of the following groups:
|type|description|
|----|-----------|
|Administrators|These users have the most privileges. They can change any system configuration parameter and access any file in the system.|
|Standard Users|These users can access the computer but only perform limited tasks. Typically these users can not make permanent or essential changes to the system and are limited to their files.|

Any user with administrative privileges will be part of the Administrators group. On the other hand, standard users are part of the Users group.

In addition to that, you will usually hear about some special built-in accounts used by the operating system in the context of privilege escalation:
|type|description|
|----|-----------|
|SYSTEM / LocalSystem|An account used by the operating system to perform internal tasks. It has full access to all files and resources available on the host with even higher privileges than administrators.|
|Local Service|Default account used to run Windows services with "minimum" privileges. It will use anonymous connections over the network.|
|Network Service|Default account used to run Windows services with "minimum" privileges. It will use the computer credentials to authenticate through the network.|

