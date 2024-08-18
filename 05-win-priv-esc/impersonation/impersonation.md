## Token Impersonation

Tokens - temp keys that allow you to access a sys/network without having to provide credentials each time you access a file; think cookie for computers
- Delegate : created for logging into the machine or using remote desktop
- Impersonate : non-interactive such as attaching network drive or domain login script


We get a foothold and we can run the following to see our privileges
> whoami /priv

SeImpersonatePrivlege - can do token impersonation

see more here - https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/#eop-impersonation-privileges

