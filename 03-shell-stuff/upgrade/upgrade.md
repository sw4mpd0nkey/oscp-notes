## Upgrading your shell

### Simple

> python3 -c 'import pty; pty.spawn("/bin/bash")'

### More Adv

```
#### In reverse shell
$ python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

#### In Kali
$stty raw -echo
$ reset
$ fg

# In reverse shell
$ export TERM=screen
```