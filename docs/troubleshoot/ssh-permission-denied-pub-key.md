# Permission denied (publickey)

During ssh to the VM, or VPS we can get this error:
```bash
root@devserver1.<my-domain.com>: Permission denied (publickey).
```

## Add ssh keys to ssh agent and try again

### Ubuntu
```bash
ssh-add ~/.ssh/<root-ssh-key>
```
```bash
ssh-add ~/.ssh/<vm-user-key>
```

### CachyOS
```bash
ssh-agent fish
```
Or, [Change shell to bash](change-shell-to-bash.md)

```bash
ssh-add ~/.ssh/<root-ssh-key>
```
```bash
ssh-add ~/.ssh/<vm-user-key>
```

Try again
```bash
ssh root@<virtual-m-ip>
```
```bash
ssh vmuser@<virtual-m-ip>
```

