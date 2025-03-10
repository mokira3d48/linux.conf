# Docker
## Installing
### Ubuntu
```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Download the latest DEB package of docker at [this link](https://docs.docker.com/desktop/setup/install/linux/ubuntu/#install-docker-desktop). And then, run the following command lines to install it.

```shell
 sudo apt-get update
 sudo apt-get install ./docker-desktop-amd64.deb
```

Verify the verison, to check if docker is correctly installed.

```shell
docker --version
```

List the image available on your computer.

```shell
docker image ls
```

If your output is like this:

```
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
```

To fix this bugs, we can run the following command lines:

```shell
sudo systemctl start docker
sudo chmod 777 /var/run/docker.sock
```

To list containers available on your computer, run the following command line:

```shell
docker container ls
```

