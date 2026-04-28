## Install Canonical Multipass

### Install snapd on CachyOS
```bash
paru snapd
```
This will list software from the aur repository. Choose, and enter the number listing aur/snapd. I enter 1 and continue
```bash
1 aur/snapd 2.74.1-1 [+229 ~2.16] [Installed]
    Service and tools for management of snap packages.
...
:: Packages to install (eg: 1 2 3, 1-3):
:: 1
```

```bash
Aur (1)    Old Version  New Version    Make Only
aur/snapd  2.70-3       2.74.1-1         No

:: Proceed to review? [Y/n]: Y
```
May ask for user password. 

```bash
sudo systemctl enable --now snapd.socket
sudo systemctl enable --now snapd.apparmor.service
sudo ln -s /var/lib/snapd/snap /snap
```

### Install Multipass

#### Install packages - Ubuntu
```bash
sudo apt install lego nano curl python3-pip
```

#### Install packages - CachyOS
```bash
sudo pacman -S lego nano curl python-pip base-devel
```

#### Install Multipass
```bash
sudo snap install multipass
```

Close open terminal windows. Open new terminal window, add ssh-keys, and continue.

### Create a Virtual Machine with Canonical Multipass

```bash
mkdir -p < coolify-app-full-path >/multipass/cloud-init
```

Create multipass/cloud-init/cloud-config.yaml file. Or copy multipass/cloud-init/cloud-config.yaml.example as multipass/cloud-init/cloud-config.yaml and edit the file.

```yaml
#cloud-config

# timedatectl list-timezones  command lists the timezones.
# Select your zone from the list , and enter the value to the name key below.
# timezone: "US/Eastern"
timezone: "Utc"
# timezone: "Europe/Istanbul"

no_ssh_fingerprints: false

ssh:
  allow_public_ssh_keys: true
  emit_keys_to_console: false

# Basic system setup
hostname: devserver1
fqdn: devserver1.my-server.com

disable_root: false

groups:
  - admingroup:
    - root
    - sys
  - cloud-users

# User setup configuration
users:
  - default
  - name: vmuser
    gecos: Development User
    primary_group: vmuser
    groups: users, sudo
    homedir: /home/vmuser
    shell: /bin/bash
    lock_passwd: true
    # Enter the public ssh key below. Copy value from ~/.ssh/vmuserkey.pub 
    ssh_authorized_keys:
    - "ssh-ed25519 Enter_your_vmuserkey.pub_content_inside_quotes vmuser"
    sudo: ['ALL=(ALL) NOPASSWD:ALL']

  - name: root
    gecos: Root User
    primary_group: root
    homedir: /root
    shell: /bin/bash
    lock_passwd: true
    # Enter the public ssh key below. Copy value from ~/.ssh/coolifyrootkey.pub
    ssh_authorized_keys:
    - "ssh-ed25519 Enter_your_vmuserkey.pub_content_inside_quotes root"

# https://cloudinit.readthedocs.io/en/latest/reference/modules.html#locale
locale: false

keyboard:
  layout: us
  model: pc105
  variant: nodeadkeys
  options: compose:rwin

package_update: true
package_upgrade: true
packages:
  - lego
  - nano
  - curl
  - python3-pip
  - ncdu

# Reboot the instance after configuration
power_state:
  mode: reboot
  message: Rebooting after initial setup
  timeout: 30
  condition: true
```

The following values must be modified according to your setup
```yaml
#cloud-config
.....
timezone: "Utc"
.....
hostname: devserver1
fqdn: devserver1.myserver.com

# User setup configuration
users:
  - name: vmuser
    .....
    # Enter the public ssh key below. Copy value from ~/.ssh/vmuserkey.pub 
    ssh_authorized_keys:
    - "paste_value_here"
.....
  - name: root
    .....
    # Enter the public ssh key below. Copy value from ~/.ssh/coolifyrootkey.pub
    ssh_authorized_keys:
    - "paste_value_here"
.....
keyboard:
  layout: en
  model: pc105
  variant: nodeadkeys
  options: compose:rwin
```


### Create the Virtual Machine

```bash
cd < coolify-app-full-path >/multipass/cloud-init
```
```bash
multipass launch 24.04 --name coolvm --cpus 2 --disk 40G --memory 8G --cloud-init cloud-config.yaml
```
This will take some time. Also may give an error, "launch failed: The following errors occurred:
timed out waiting for initialization to complete", but still work.

```bash
multipass list
```
![ip address](images/vm_ip_address.png)

Note the IP address in the output ( IPv4 ) . We will use it in the following steps. It takes some time for VM to start. If VM state is "Restarting" then wait a few seconds, and run the command again.

Continue from: [Test Virtual Machine ssh login](test-ssh-dev.md)

Back to [Development Environment Installation](development.md)
