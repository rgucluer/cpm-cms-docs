## Create & Add ssh keys

https://coolify.io/docs/knowledge-base/server/openssh

From documentation: The SSH key must not have a passphrase or 2FA enabled for the user used to run the Coolify installation script or the connection will fail. 

But I used a ssh key protected by a passphraze, and run the script. It did not fail. But I added the ssh key to ssh agent before I run the installation script.


```bash
cd ~/.ssh
```

```bash
ssh-agent bash
```

```bash
ssh-keygen -C vmuser -f vmuserkey
```

It will ask for a passphraze, you can enter one, or skip it by pressing <kbd>ENTER</kbd> . ( You will need this passphraze later, keep it safe. )

Make key pair readable/writable only by the user: 
```bash
chmod u=wr-,g=---,o=--- ~/.ssh/vmuserkey*
```
Don't forget the "*" at the end.


### Add ssh key to ssh agent:
```bash
ssh-add ~/.ssh/vmuserkey
```


### create an ssh key for VM root user
```bash
ssh-keygen -C root -f coolifyrootkey
```

It will ask for a passphraze, you can enter one, or skip it by pressing <kbd>ENTER</kbd> . ( You will need this passphraze later, keep it safe. )

Make key pair readable/writable only by the user: 
```bash
chmod u=wr-,g=---,o=--- ~/.ssh/coolifyrootkey*
```
Don't forget the "*" at the end.

### Add ssh key to ssh agent for Coolify VM root user
```bash
ssh-add ~/.ssh/coolifyrootkey
```
