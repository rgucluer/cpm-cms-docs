## Edit /etc/hosts file
On Developer Pc, add the following rows to the /etc/hosts file with a text editor. You must run the text editor with sudo privileges. virtual-m-ip is the IP we noted before (multipass list).
```bash
sudo nano /etc/hosts
```
Add the following rows to the file.
```txt
.....
< virtual-m-ip > www.< dev-domain-name >
< virtual-m-ip > traefik.< dev-domain-name > coolify.< dev-domain-name >
< virtual-m-ip > < dev-domain-name >
< virtual-m-ip > < vm-server-name >
```
<kbd>CTRL</kbd> + <kbd>o</kbd> to Save.

<kbd>CTRL</kbd> + <kbd>x</kbd> to Exit.

---

### Continue with, Test Virtual Machine ssh login [multipass/test-ssh-dev](install-multipass-create-vm.md#test-virtual-machine-ssh-login-multipasstest-ssh-dev)
