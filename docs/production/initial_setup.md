# Initial Setup of a Virtual Private Server

Read documentation of your VPS Service Provider, and make root user able to login via ssh without a password (via public key) .

## [Create & Add ssh keys](create-ssh-keys-prod.md)

- Update apt packages

## Install apt packages
Install packages
- lego
- nano
- curl
- python3-pip
- ncdu

```bash
sudo apt install lego nano curl python3-pip ncdu
```

## [Install git](../install-git.md)

## Set hostname on VPS

```bash
hostnamectl set-hostname <server-name>
```

```bash
hostnamectl set-hostname serv1
```

```bash
nano /etc/hosts
```

```bash
127.0.1.1 traefik.<domain-name> coolify.<domain-name> www.<domain-name> <servername>.<domain-name> <domain-name> <servername>
127.0.0.1 localhost
.....
```
Save, and exit.

Then, restart the SSH service and the new hostname is set:
```bash
systemctl restart ssh
```

Example: serv1.my-domain.com


## Change timezone

https://linuxconfig.org/ubuntu-24-04-change-timezone

Status
```bash
timedatectl
```

Find your Timezone
```bash
timedatectl list-timezones
```

Use up/down arrow keys, PgUp/PgDown keys. Search for your timezone. Copy or note the string.


Set New Timezone
```bash
timedatectl set-timezone <timezone>
```

Edit /etc/ssh/sshd_config

```bash
.....
# Set your ssh port here, Open this port in your firewall
Port 22
MaxAuthTries 200
PermitRootLogin prohibit-password
PubkeyAuthentication yes
#PasswordAuthentication no
KbdInteractiveAuthentication no
UsePAM yes
X11Forwarding no
.....
# TCPKeepAlive yes
# ClientAliveInterval 60
# ClientAliveCountMax 3
.....
# AllowUsers <vps-user-name> root

```

```bash
systemctl restart ssh
```

It would be better if we can use Coolify with a regular user.

