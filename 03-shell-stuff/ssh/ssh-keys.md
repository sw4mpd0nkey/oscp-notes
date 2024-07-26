## SSH Keys

Creating ssh keys are uploading them onto a target or pulling down existing keys are a fun sneaky way to get to your goal, plus going through the front door has some perks as well such as not needing to sacrifice a small child to get the shell to behave correctly.

### Loading onto Host

```
$ ssh-keygen

Keys will be saved to
/home/yourusername/.ssh/id_rsa <- private key
/home/yourusername/.ssh/id_rsa.pub <- public key
```

1. Copy the contents of the public key (id_rsa.pub)
2. Save the contents on the victim machine in the authorized_keys file (if it does not exist, create it) authorized_keys location:

```
/home/user/.ssh/authorized_keys

root's authorized_keys location
/root/.ssh/authorized_keys
```

3. Once the key is saved in the authorized_keys, you will not need a password to sign in

Use your private key (generated with the public key using ssh-keygen) to sign in:

Make sure to update the correct key-permissions first:

```
$ chmod 600 id_rsa

$ ssh -i id_rsa user@host
```
