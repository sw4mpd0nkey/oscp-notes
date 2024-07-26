## Rando Fixes for SSh


### Too many authentication failures
If you see this error message:
```
Received disconnect from <server ip> port 22:2: Too many authentication failures
Disconnected from <server ip> port 22
```
You can clean up all keys with the following as a quick and dirty solution:
```
ssh-add -D
```
You can also change the parameter #MaxAuthTries 6 in sshd_config of the specific server
