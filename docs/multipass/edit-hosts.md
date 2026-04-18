## Edit /etc/hosts file
On Developer Pc, add the following rows to the /etc/hosts file with a text editor. You must run the text editor with sudo privileges. virtual-m-ip is the IP we noted before (multipass list).
```bash
sudo nano /etc/hosts
```
Add the following rows to the file.
```txt
.....
<virtual-m-ip> www.devserver1.my-domain.com 
<virtual-m-ip> traefik.devserver1.my-domain.com coolify.devserver1.my-domain.com 
<virtual-m-ip> devserver1.my-domain.com 
<virtual-m-ip> devserver1
```
<kbd>CTRL</kbd> + <kbd>o</kbd> to Save.

<kbd>CTRL</kbd> + <kbd>x</kbd> to Exit.

Web browser is directed to this virtual machine when we enter http://<my-domain> to the address bar. We will do it later, not now.

Continue with: [Test Virtual Machine ssh login with domain name](test-ssh-dev-domain.md)
