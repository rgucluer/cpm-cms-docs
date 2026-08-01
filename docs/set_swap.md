# Set swap

- Connect to the computer where you want ot set swap

## Check swap files

```bash
free -h
```

```bash
               total        used        free      shared  buff/cache   available
Mem:            15Gi       1.3Gi        13Gi       198Mi       1.6Gi        14Gi
Swap:             0B          0B          0B
```

## Check swappiness

```bash
sudo swapon --show
```

No Result, or similar to

```bash
NAME      TYPE SIZE USED PRIO
/swapfile file   4G   0B   -2
```

or

```bash
NAME       TYPE       SIZE USED PRIO
/dev/zram0 partition 31,2G   0B  100
```

or

```bash
NAME      TYPE       SIZE USED PRIO
/dev/dm-1 partition 21.2G   0B   -2
```


## Creating a Swap File
Create a swap file if one does not exist
```bash
sudo fallocate -l 4G /swapfile
```

```bash
ls -lh /swapfile
```

```bash
-rw------- 1 root root 4.0G Jan  6 00:21 /swapfile
```

## Enabling the Swap File
```bash
chmod 600 /swapfile
```

```bash
ls -lh /swapfile
```

```bash
-rw------- 1 root root 4.0G Jan  6 00:21 /swapfile
```

```bash
sudo mkswap /swapfile
```

```bash
Setting up swapspace version 1, size = 4 GiB (4294963200 bytes)
no label, UUID=.....
```

```bash
sudo swapon /swapfile
```

```bash
sudo swapon --show
```

```bash
NAME      TYPE SIZE USED PRIO
/swapfile file   4G   0B   -2
```

```bash
free -h
```

```bash
               total        used        free      shared  buff/cache   available
Mem:            15Gi       1.3Gi        12Gi       200Mi       2.2Gi        14Gi
Swap:          4.0Gi          0B       4.0Gi
```

## Making the Swap File Permanent
```bash
sudo cp /etc/fstab /etc/fstab.bak
```

```bash
sudo echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

## Tuning your Swap Settings

There are a few options that you can configure that will have an impact on your system's performance when dealing with swap.

### Adjusting the Swappiness Property

```bash
cat /proc/sys/vm/swappiness
```

```bash
sudo sysctl vm.swappiness=1
```
If you have low physical memory, do not set swappiness to a low value. 
Also check swap status on Virtual Machine, and Virtual Private Server. 

---

## For development environment continue with: 
### Install Multipass and Create a Virtual Machine [multipass/install-multipass-create-vm](install-cpm-cms-dev.md#install-multipass-and-create-a-virtual-machine-multipassinstall-multipass-create-vm) .

---

## For production environment continue with:
### Install & run Coolify on a Virtual Private Server [coolify/install-prod-coolify](install-cpm-cms-prod.md#install--run-coolify-on-a-virtual-private-server-coolifyinstall-prod-coolify)

---

## References:
- Digital Ocean tutorial: 
  - https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04#step-4-enabling-the-swap-file
 
- Coolify discussions
  - https://github.com/coollabsio/coolify/discussions/2754

- https://documentation.ubuntu.com/multipass/latest/reference/settings/
  - Please read the documentation. Thank you Brian
