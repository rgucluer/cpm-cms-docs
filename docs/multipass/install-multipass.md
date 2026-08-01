## Install Canonical Multipass

#### Install Ubuntu, Debian, PopOS
```bash
sudo apt install nano curl python3-pip snapd
```

### Install Multipass
```bash
sudo snap install multipass
```

```bash
sudo systemctl restart snapd
```

```bash
sudo systemctl daemon-reload
```

- Add the directory to your shell profile
  - (~/.bashrc or similar file depends on your distribution)

```bash
export PATH="$PATH:/snap/bin" 
```  

Close, and reopen terminal windows. Add ssh-keys, and continue.

---

### Continue with: Create the Virtual Machine [multipass/create-virtual-machine](install-multipass-create-vm.md#create-the-virtual-machine-multipasscreate-virtual-machine)

