# Install Docker on Development PC

## How to install Docker Engine on Debian 13

Following documentation https://docs.docker.com/engine/install/debian/ .

### Prerequisites

#### Uninstall old versions

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-doc podman-docker containerd runc | cut -f1)
```

- Installation steps

#### Add Docker's official GPG key:
```bash
sudo apt update
```
```bash
sudo apt install ca-certificates curl
```
```bash
sudo install -m 0755 -d /etc/apt/keyrings
```
```bash
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
```
```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
#### Add the repository to Apt sources:
```bash
$ sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
```
```bash
> Types: deb
```
```bash
> URIs: https://download.docker.com/linux/debian
```
```bash
> Suites: trixie
```
```bash
> Components: stable
```
```bash
> Architectures: amd64
```
```bash
> Signed-By: /etc/apt/keyrings/docker.asc
```
```bash
> EOF
```
```bash
$ sudo apt update
```
#### Install 
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### Check
```bash
sudo systemctl status docker
```

#### Verify that the installation is successful by running the hello-world image:
```bash
sudo docker run hello-world
```

https://docs.docker.com/engine/install/linux-postinstall/

#### Add system user to docker group
```bash
sudo usermod -aG docker $USER
```
- Log out and log back in so that your group membership is re-evaluated.
- You can also run the following command to activate the changes to groups:
  - ```bash
    newgrp docker
    ```




Note:
- Arch is not among supported Linux Distributions

https://docs.docker.com/engine/install/

https://docs.docker.com/engine/install/debian/

https://docs.docker.com/build/buildkit/

https://mobyproject.org/

https://github.com/moby/moby#readme
