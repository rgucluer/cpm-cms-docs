
ssh to VM or VPS with root user.

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
swapon --show
```

No Result, or

```bash
NAME      TYPE SIZE USED PRIO
/swapfile file   4G   0B   -2
```

or

```bash
NAME       TYPE       SIZE USED PRIO
/dev/zram0 partition 31,2G   0B  100
```

## Creating a Swap File
Create a swap file one does not exist
```bash
sudo fallocate -l 4G /swapfile
```

```bash
ls -lh /swapfile
```

```bash
-rw------- 1 root root 4.0G Jan  6 00:21 /swapfile
```

TODO: Check file ownership information (root:disk on CachyOS)

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
mkswap /swapfile
```

```bash
Setting up swapspace version 1, size = 4 GiB (4294963200 bytes)
no label, UUID=.....
```

```bash
swapon /swapfile
```

```bash
swapon --show
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
cp /etc/fstab /etc/fstab.bak
```

```bash
echo '/dev/zram0 none swap sw 0 0' | sudo tee -a /etc/fstab
```

## Tuning your Swap Settings

There are a few options that you can configure that will have an impact on your system’s performance when dealing with swap.

### Adjusting the Swappiness Property

```bash
cat /proc/sys/vm/swappiness
```

```bash
sudo sysctl vm.swappiness=1
```

Also check swap status on Virtual Machine, and Virtual Private Server

### Continue

For development environment continue with [Install Multipass and Create a Virtual Machine](multipass/install-dev-multipass.md) .

For production environment continue with [Install & run Coolify on a Virtual Private Server](coolify/install-prod-coolify.md)

### References:
- Digital Ocean tutorial: 
  - https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04#step-4-enabling-the-swap-file
 
- Coolify discussions
  - https://github.com/coollabsio/coolify/discussions/2754

- https://documentation.ubuntu.com/multipass/latest/reference/settings/
  - Please read the documentation. Thank you Brian
