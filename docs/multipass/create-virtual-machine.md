## Create a Virtual Machine with Canonical Multipass

- We use the ssh keys we created before

Copy multipass/cloud-init/cloud-config.yaml.example as multipass/cloud-init/cloud-config.yaml and edit the file.

```yaml
#cloud-config

# timedatectl list-timezones  command lists the timezones.
# Select your zone from the list , and enter the value to the name key below.
# timezone: "US/Eastern"
timezone: "Utc"
# timezone: "Europe/Istanbul"

no_ssh_fingerprints: false

# https://docs.cloud-init.io/en/latest/reference/yaml_examples/ssh.html
ssh:
  emit_keys_to_console: false

# Basic system setup

# < vm-server-name >
hostname: devserver1

# < vm-server-name >.< domain-name >
fqdn: devserver1.my-domain.com

disable_root: false

groups:
  - admingroup:
    - root
    - sys
  - cloud-users

# User setup configuration
users:
  - default
    # < vm-user-name >
  - name: vmuser
    gecos: Development User
    # < vm-user-name >
    primary_group: vmuser
    groups: users, sudo
    # /home/< vm-user-name >
    homedir: /home/vmuser
    shell: /bin/bash
    lock_passwd: true
    # Enter the public ssh key below. Copy value from ~/.ssh/< vm-user-key >.pub 
    ssh_authorized_keys:
    - "ssh-ed25519 Enter_your_< vm-user-name >.pub_content_inside_quotes < vm-user-name >"
    sudo: ['ALL=(ALL) NOPASSWD:ALL']

  - name: root
    gecos: Root User
    primary_group: root
    homedir: /root
    shell: /bin/bash
    lock_passwd: true
    # Enter the public ssh key below. Copy value from ~/.ssh/< vm-coolify-rootkey >.pub
    ssh_authorized_keys:
    - "ssh-ed25519 Enter_your_< vm-coolify-rootkey >.pub_content_inside_quotes root"

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

Be sure to change values; fqdn, users.vmuser.ssh_authorized_keys, users.root.ssh_authorized_keys

### Create the Virtual Machine

```bash
cd < cpm-cms-docs >/multipass/cloud-init
```

< vm-name > : coolvm

```bash
multipass launch 24.04 --name coolvm --cpus 2 --disk 40G --memory 8G --cloud-init cloud-config.yaml
```
This will take some time. Also may give an error, "launch failed: The following errors occurred:
timed out waiting for initialization to complete", but still work.

```bash
multipass list
```
Note the IP address in the output ( IPv4 ) . We will use it in the following steps. It takes some time for VM to start. If VM state is "Restarting" then wait a few seconds, and run the command again.

---

### Continue with, Edit /etc/hosts file on Developer PC [multipass/install-multipass-create-vm](install-multipass-create-vm.md#edit-etchosts-file-on-developer-pc-multipassedit-hosts)

---

