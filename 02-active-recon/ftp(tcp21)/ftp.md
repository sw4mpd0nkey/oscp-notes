# FTP Enumeration

NOTE: always try logging in as admin:admin

## Banner Grabbing

Will print any banner info
> nc -vn *\<IP\>* 21

Get certificate if any
> openssl s_client -connect *\<IP\>*:21 -starttls ftp 

## Unauth enum

With nmap
> sudo nmap -sV -p21 -sC -A *\<IP\>*

You can use the commands HELP and FEAT to obtain some information of the FTP server:
```
HELP
214-The following commands are recognized (* =>'s unimplemented):
214-CWD     XCWD    CDUP    XCUP    SMNT*   QUIT    PORT    PASV    
214-EPRT    EPSV    ALLO*   RNFR    RNTO    DELE    MDTM    RMD     
214-XRMD    MKD     XMKD    PWD     XPWD    SIZE    SYST    HELP    
214-NOOP    FEAT    OPTS    AUTH    CCC*    CONF*   ENC*    MIC*    
214-PBSZ    PROT    TYPE    STRU    MODE    RETR    STOR    STOU    
214-APPE    REST    ABOR    USER    PASS    ACCT*   REIN*   LIST    
214-NLST    STAT    SITE    MLSD    MLST    
214 Direct comments to root@drei.work

FEAT
211-Features:
 PROT
 CCC
 PBSZ
 AUTH TLS
 MFF modify;UNIX.group;UNIX.mode;
 REST STREAM
 MLST modify*;perm*;size*;type*;unique*;UNIX.group*;UNIX.mode*;UNIX.owner*;
 UTF8
 EPRT
 EPSV
 LANG en-US
 MDTM
 SSCN
 TVFS
 MFMT
 SIZE
211 End

STAT
#Info about the FTP server (version, configs, status...)
```

## Anonymous login

possible credentials:
```
anonymous : anonymous
anonymous :
ftp : ftp
```

Anonymous connection:
> ftp *\<IP\>*
> anonymous
> anonymous

List all files (even hidden) (yes, they could be hidden)
> ls -a 

Set transmission to binary instead of ascii
> binary

Set transmission to ascii instead of binary 
> ascii 

exit
> bye

## Brute Force

Here you can find a nice list with default ftp credentials: https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt

> hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt *\<IP\>* ftp

## Download all files from FTP

Donwload all
> wget -m ftp://anonymous:anonymous@*\<IP\>*

Download all
> wget -m --no-passive ftp://anonymous:anonymous@*\<IP\>*

## Password Spray
> hydra -L users -P /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt 192.168.191.46 ftp -V -f