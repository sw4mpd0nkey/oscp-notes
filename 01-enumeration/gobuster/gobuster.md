# GoBuster

## Directory Search

The DIR mode is used for finding hidden directories and files. 

> gobuster dir -u *\<url\>* -w *\<wordlist\>* -o *\<hostname\>*-dir --exclude-length 0

## DNS Search

Use the DNS command to discover subdomains with Gobuster. To see the options and flags available specifically for the DNS command use: gobuster dns --help

> gobuster dns -d mydomain.com -w /usr/share/wordlists/dirb/common.txt -o *\<hostname\>*-dns


## VHost Search

The vhost command discovers Virtual host names on target web servers. Virtual hosting is a technique for hosting multiple domain names on a single server. Exposing hostnames on a server may reveal supplementary web content belonging to the target. Vhost checks if the subdomains exist by visiting the formed URL and cross-checking the IP address.

> gobuster vhost -u https://example.com -t 50 -w /wordlists/Discovery/DNS/subdomains-top1million-5000.txt -o *\<hostname\>*-vhost --exclude-length 0


## word lists to consider
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt