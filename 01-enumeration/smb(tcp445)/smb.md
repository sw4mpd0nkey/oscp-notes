## Generic SMB Enum Notes

The NetBIOS service listens on TCP port 139, as well as several UDP ports. It should be noted that SMB (TCP port 445) and NetBIOS are two separate protocols. NetBIOS is an independent session layer protocol and service that allows computers on a local network to communicate with each other. While modern implementations of SMB can work without NetBIOS, NetBIOS over TCP (NBT) is required for backward compatibility and these are often enabled together. This also means the enumeration of these two services often goes hand-in-hand. These services can be scanned with tools like nmap, using syntax similar to the following:

> nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254

We can use this to query the NetBIOS name service for valid NetBIOS names, specifying the originating UDP port as 137 with the -r option.

```
kali@kali:~$ sudo nbtscan -r 192.168.50.0/24
Doing NBT name scan for addresses from 192.168.50.0/24

IP address       NetBIOS Name     Server    User             MAC address
------------------------------------------------------------------------------
192.168.50.124   SAMBA            <server>  SAMBA            00:00:00:00:00:00
192.168.50.134   SAMBAWEB         <server>  SAMBAWEB         00:00:00:00:00:00
...


Single host OS Discovery
> nmap -v -p 139,445 --script smb-os-discovery 192.168.50.152

Listing shares
> smbmap -H 192.168.158.121

Vulnerability Scans
> nmap -p 139,445 --script smb-vuln* 192.168.199.240


download directory
> smbget --recursive --no-pass smb://server/share

> smbget smb://server/share/file --user username%password