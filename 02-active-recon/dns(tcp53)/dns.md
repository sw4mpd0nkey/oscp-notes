## DNS Enumeration

### Whois

> whois <domain>

### Nmap 

```
nmap -sC -sV -p53 $ip/24

nmap -p 80 --script dns-brute.nse domain.com
# Find DNS (A) records by trying a list of common sub-domains from a wordlist.

nmap $ip --script=dns-zone-transfer -p 53
# Zone transfer script
```

### Host

#### Domain scan

```
$ host google.com

google.com has address 142.250.183.78
google.com has IPv6 address 2404:6800:4009:822::200e
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
```

#### Find particular records 

```
host -t mx google.com
# Return mail servers 

host -t ns google.com
# Return name servers 

host -t txt google.com
# Return txt records
```

#### Reverse Domain Lookup

```
$ host gnu.org        

gnu.org has address 209.51.188.148
gnu.org has IPv6 address 2001:470:142:3::a
gnu.org mail is handled by 10 eggs.gnu.org.

$ host 209.51.188.148

148.188.51.209.in-addr.arpa is an alias for 148.0-24.188.51.209.in-addr.arpa.
148.0-24.188.51.209.in-addr.arpa domain name pointer wildebeest.gnu.org.
```

### DNS Zone Transfer

DNS zone transfer, also known as DNS query type AXFR, is a process by which a DNS server passes a copy of part of its database to another DNS server. The portion of the database that is replicated is known as a zone.

host -l <domain> <NameServer>

$ host -t ns zonetransfer.me               # first list out their name servers to check for zone transfer

zonetransfer.me name server nsztm2.digi.ninja.
zonetransfer.me name server nsztm1.digi.ninja.

$ host -l zonetransfer.me nsztm1.digi.ninja

Using domain server:
Name: nsztm1.digi.ninja
Address: 81.4.108.41#53
Aliases: 

zonetransfer.me has address 5.196.105.14
zonetransfer.me name server nsztm1.digi.ninja.
zonetransfer.me name server nsztm2.digi.ninja.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me domain name pointer www.zonetransfer.me.
asfdbbox.zonetransfer.me has address 127.0.0.1
canberra-office.zonetransfer.me has address 202.14.81.230
dc-office.zonetransfer.me has address 143.228.181.132
deadbeef.zonetransfer.me has IPv6 address dead:beaf::
email.zonetransfer.me has address 74.125.206.26
home.zonetransfer.me has address 127.0.0.1
internal.zonetransfer.me name server intns1.zonetransfer.me.
internal.zonetransfer.me name server intns2.zonetransfer.me.
intns1.zonetransfer.me has address 81.4.108.41
intns2.zonetransfer.me has address 167.88.42.94
office.zonetransfer.me has address 4.23.39.254
ipv6actnow.org.zonetransfer.me has IPv6 address 2001:67c:2e8:11::c100:1332
owa.zonetransfer.me has address 207.46.197.32
alltcpportsopen.firewall.test.zonetransfer.me has address 127.0.0.1
vpn.zonetransfer.me has address 174.36.59.154
www.zonetransfer.me has address 5.196.105.14

Zone Transfer Script

#!/bin/bash
# Simple Zone Transfer Bash Script
# $1 is the first argument given after the bash script
# Check if argument was given, if not, print usage
if [ -z "$1" ]; then
echo "[*] Simple Zone transfer script"
echo "[*] Usage : $0 <domain name> "
exit 0
fi
# if argument was given, identify the DNS servers for the domain
for server in $(host -t ns $1 | cut -d " " -f4); do
# For each of these servers, attempt a zone transfer
host -l $1 $server |grep "has address"
done

Subdomain bruteforcing using common hostname

for ip in $(cat list.txt); do host $ip.website.com; done

Reverse dns lookup bruteforcing

for ip in $(seq 155 190);do host 50.7.67.$ip;done |grep -v "not found"

The ip is based on subdomain bruteforcing result
Nslookup

    nslookup is used to query Internet name servers interactively

$ nslookup hsploit.com

Server:     203.153.41.28
Address:    203.153.41.28#53

Non-authoritative answer:
Name:   hsploit.com
Address: 104.21.38.165
Name:   hsploit.com
Address: 172.67.136.119
Name:   hsploit.com
Address: 2606:4700:3033::6815:26a5
Name:   hsploit.com
Address: 2606:4700:3035::ac43:8877

Running in intrective mode

$ nslookup

> set type=ns
> hsploit.com
Server:     203.153.41.28
Address:    203.153.41.28#53

Non-authoritative answer:
hsploit.com nameserver = dee.ns.cloudflare.com.
hsploit.com nameserver = jim.ns.cloudflare.com.

Authoritative answers can be found from:
> 

Gathering information from specific DNS server

$ nslookup                                

> server 10.10.10.13
Default server: 10.10.10.13
Address: 10.10.10.13#53

> 10.10.10.13
13.10.10.10.in-addr.arpa    name = ns1.cronos.htb.

Dig
Domain scan 

$ dig hsploit.com

; <<>> DiG 9.16.18 <<>> hsploit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 13539
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;hsploit.com.           IN  A

;; ANSWER SECTION:
hsploit.com.        1200    IN  A   104.21.38.165
hsploit.com.        1200    IN  A   172.67.136.119

;; Query time: 139 msec
;; SERVER: 203.153.41.28#53(203.153.41.28)
;; WHEN: Thu Jul 22 17:04:58 IST 2021
;; MSG SIZE  rcvd: 72

Asking for particular records

dig hsploit.com -t mx

Sorting the output

$ dig hsploit.com -t ns +short 

dee.ns.cloudflare.com.
jim.ns.cloudflare.com.

Note - if particular type of information is not available , dig will give no output so don't think at that time that tool is not working xD
Reverse Domain Lookup

$ dig -x 142.250.183.78 +short        

bom12s12-in-f14.1e100.net.

Zone Transfer

$ dig zonetransfer.me ns +short 

nsztm2.digi.ninja.
nsztm1.digi.ninja.

$ dig axfr zonetransfer.me @nsztm1.digi.ninja

; <<>> DiG 9.16.18 <<>> axfr zonetransfer.me @nsztm1.digi.ninja
;; global options: +cmd
zonetransfer.me.    7200    IN  SOA nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.    300 IN  HINFO   "Casio fx-700G" "Windows XP"
zonetransfer.me.    301 IN  TXT "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.    7200    IN  MX  0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.    7200    IN  MX  10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me.    7200    IN  MX  10 ALT2.ASPMX.L.GOOGLE.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  A   5.196.105.14
zonetransfer.me.    7200    IN  NS  nsztm1.digi.ninja.
zonetransfer.me.    7200    IN  NS  nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me. 301 IN TXT "6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN SRV 0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200 IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN   AFSDB   1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200  IN  A   127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN    AFSDB   1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A  202.14.81.230
cmdexec.zonetransfer.me. 300    IN  TXT "; ls"
contact.zonetransfer.me. 2592000 IN TXT "Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
dc-office.zonetransfer.me. 7200 IN  A   143.228.181.132
deadbeef.zonetransfer.me. 7201  IN  AAAA    dead:beaf::
dr.zonetransfer.me. 300 IN  LOC 53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
DZC.zonetransfer.me.    7200    IN  TXT "AbCdEfG"
email.zonetransfer.me.  2222    IN  NAPTR   1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
email.zonetransfer.me.  7200    IN  A   74.125.206.26
Hello.zonetransfer.me.  7200    IN  TXT "Hi to Josh and all his class"
home.zonetransfer.me.   7200    IN  A   127.0.0.1
Info.zonetransfer.me.   7200    IN  TXT "ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
internal.zonetransfer.me. 300   IN  NS  intns1.zonetransfer.me.
internal.zonetransfer.me. 300   IN  NS  intns2.zonetransfer.me.
intns1.zonetransfer.me. 300 IN  A   81.4.108.41
intns2.zonetransfer.me. 300 IN  A   167.88.42.94
office.zonetransfer.me. 7200    IN  A   4.23.39.254
ipv6actnow.org.zonetransfer.me. 7200 IN AAAA    2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.    7200    IN  A   207.46.197.32
robinwood.zonetransfer.me. 302  IN  TXT "Robin Wood"
rp.zonetransfer.me. 321 IN  RP  robin.zonetransfer.me. robinwood.zonetransfer.me.
sip.zonetransfer.me.    3333    IN  NAPTR   2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
sqli.zonetransfer.me.   300 IN  TXT "' or 1=1 --"
sshock.zonetransfer.me. 7200    IN  TXT "() { :]}; echo ShellShocked"
staging.zonetransfer.me. 7200   IN  CNAME   www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A 127.0.0.1
testing.zonetransfer.me. 301    IN  CNAME   www.zonetransfer.me.
vpn.zonetransfer.me.    4000    IN  A   174.36.59.154
www.zonetransfer.me.    7200    IN  A   5.196.105.14
xss.zonetransfer.me.    300 IN  TXT "'><script>alert('Boo')</script>"
zonetransfer.me.    7200    IN  SOA nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
;; Query time: 133 msec
;; SERVER: 81.4.108.41#53(81.4.108.41)
;; WHEN: Thu Jul 22 17:28:02 IST 2021
;; XFR size: 50 records (messages 1, bytes 1994)

Automated Scanners
Fierce

fierce --domain google.com

Dnsenum

dnsenum $ip

dnsenum google.com -f /usr/share/dnsenum/dns.txt
# Brute forcing subdomains

DnsRecon

dnsrecon -d $ip

dnsrecon -d $ip -t axfr
# Perform zone transfer

dnsrecon -d $ip -D /usr/share/dnsrecon/subdomains-top1mil-20000.txt -t brt
# Perform host and subdomain brute force

Sub-Domain Enumeration
Ffuf

ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.horizontall.htb" -u http://horizontall.htb

Sublist3r

sublist3r -d <domain>
# To scan with public data

sublist3r -d <domain> -b -t 100
# To bruteforce the subdomains
# this will use following wordlist:
    /usr/share/sublist3r/subbrute/names.txt