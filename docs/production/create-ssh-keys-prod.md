## Create & Add ssh keys

https://coolify.io/docs/knowledge-base/server/openssh

From documentation: The SSH key must not have a passphrase or 2FA enabled for the user used to run the Coolify installation script or the connection will fail. 

But I used a ssh key protected by a passphraze, and run the script. It did not fail. But I added the ssh key to ssh agent before I run the installation script.

### On Developer PC

```bash
cd ~/.ssh
```

```bash
ssh-agent bash
```

```bash
ssh-keygen -C < vps-user-name > -f < vps-user-key >
```

It will ask for a passphraze, you can enter one, or skip it by pressing <kbd>ENTER</kbd> . ( You will need this passphraze later, keep it safe. )

Make key pair readable/writable only by the user: 
```bash
chmod u=wr-,g=---,o=--- ~/.ssh/< vps-user-key >*
```
Don't forget the "*" at the end.


### Add ssh key to ssh agent :
```bash
ssh-add ~/.ssh/< vps-user-key >
```

The public key must be saved in VPS, and ssh service must be configured to make root user login without a password (via the public key).