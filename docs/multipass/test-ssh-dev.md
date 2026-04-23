## Test Virtual Machine ssh login

```bash
ssh root@< virtual_machine_IP >
```

Will ask "Are you sure you want to continue connecting (yes/no/[fingerprint])?" , write  "yes" and press <kbd>ENTER</kbd> .

If you set a passphraze for your ssh key, it may ask for the passphraze.
( It won't ask for a passphraze, if you add your ssh key to ssh-agent beforehand. )

Now, we are in the Virtual Machine. To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.

Now, we are back in Controller PC .

Now, stop - start the VM and try to ssh again.

```bash
multipass stop coolvm
```

```bash
multipass start coolvm
```

```bash
ssh root@< virtual_machine_IP >
```

To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.

Do the same thing for vmuser

```bash
ssh vmuser@< virtual_machine_IP >
```

Now, we are in the Virtual Machine. To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.

Continue with [Edit /etc/hosts file on Developer PC](edit-hosts.md)


