## smbclient

### Shared Folders Enumeration
List shared folders

It is always recommended to look if you can access to anything, if you don't have credentials try using null credentials/guest user.
```
smbclient --no-pass -L //<IP> # Null user
smbclient -U 'username[%passwd]' -L [--pw-nt-hash] //<IP> #If you omit the pwd, it will be prompted. With --pw-nt-hash, the pwd provided is the NT hash

smbmap -H <IP> [-P <PORT>] #Null user
smbmap -u "username" -p "password" -H <IP> [-P <PORT>] #Creds
smbmap -u "username" -p "<NT>:<LM>" -H <IP> [-P <PORT>] #Pass-the-Hash
smbmap -R -u "username" -p "password" -H <IP> [-P <PORT>] #Recursive list

crackmapexec smb <IP> -u '' -p '' --shares #Null user
crackmapexec smb <IP> -u 'username' -p 'password' --shares #Guest user
crackmapexec smb <IP> -u 'username' -H '<HASH>' --shares #Guest user
```


### Connect/List a shared folder
```
#Connect using smbclient
smbclient --no-pass //<IP>/<Folder>
smbclient -U 'username[%passwd]' -L [--pw-nt-hash] //<IP> #If you omit the pwd, it will be prompted. With --pw-nt-hash, the pwd provided is the NT hash
#Use --no-pass -c 'recurse;ls'  to list recursively with smbclient

#List with smbmap, without folder it list everything
smbmap [-u "username" -p "password"] -R [Folder] -H <IP> [-P <PORT>] # Recursive list
smbmap [-u "username" -p "password"] -r [Folder] -H <IP> [-P <PORT>] # Non-Recursive list
smbmap -u "username" -p "<NT>:<LM>" [-r/-R] [Folder] -H <IP> [-P <PORT>] #Pass-the-Hash
```

### Download all filed in sn smb share
> smbget --recursive smb://192.168.202.248/Users --user anonymous%anonymous