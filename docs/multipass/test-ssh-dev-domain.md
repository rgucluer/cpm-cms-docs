## Test Virtual Machine ssh login with domain name


```bash
ssh root@devserver1.<my-domain.com>
```

Will ask "Are you sure you want to continue connecting (yes/no/[fingerprint])?" , write  "yes" and press <kbd>ENTER</kbd> .

If you set a passphraze for your ssh key, it may ask for the passphraze.
( It won't ask for a passphraze, if you add your ssh key to ssh-agent beforehand. )

Now, we are in the Virtual Machine. To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.

Now, we are back in Controller PC .

Do the same thing for vmuser

```bash
ssh vmuser@devserver1.<my-domain.com>
```

Now, we are in the Virtual Machine. To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.



### Troubleshooting:
- [Permission denied (publickey)](../troubleshoot//ssh-permission-denied-pub-key.md)
- [Remote host id changed](../troubleshoot/rem-host-id-change.md)
