## Test Virtual Machine ssh login with domain name


```bash
ssh root@< dev-domain-name >
``` 

Will ask "Are you sure you want to continue connecting (yes/no/[fingerprint])?" , write  "yes" and press <kbd>ENTER</kbd> .

If you set a passphraze for your ssh key, it may ask for the passphraze.
( It won't ask for a passphraze, if you add your ssh key to ssh-agent beforehand. )

Now, we are in the Virtual Machine. To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.

Now, we are back in Controller PC .

Do the same thing for vmuser

```bash
ssh vmuser@< dev-domain-name >
```

Now, we are in the Virtual Machine. To exit from ssh session, press <kbd>CTRL</kbd>+<kbd>D</kbd>.

---

### Continue with: Install & run Coolify on a local Virtual Machine [coolify/install-dev-coolify](../install-cpm-cms-dev.md#install--run-coolify-on-a-local-virtual-machine-coolifyinstall-dev-coolify)

---

### Troubleshooting:
- [Permission denied (publickey)](../troubleshoot//ssh-permission-denied-pub-key.md)
- [Remote host id changed](../troubleshoot/rem-host-id-change.md)
