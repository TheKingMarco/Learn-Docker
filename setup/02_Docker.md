# Uninstall Docker Engine on Ubuntu

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

# Installazione Docker Engine on Ubuntu
## Setup Repository
```shell
#setup repository
sudo apt-get update 
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
#aggiungiamo la key GPG ufficiale Docker
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
#setup repo
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
## Install docker engine
```shell
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 #verifica installazione
  sudo docker run hello-world
```
# Uninstall Docker Engine
```shell
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
#delete all images, volume and containers
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

# Install Docker with script (Test/Dev)
```shell
curl -fsSL https://get.docker.com -o get-docker.sh #download the script
sudo sh ./get-docker.sh --dry-run #test the script
sudo sh ./get-docker.sh #run the script
```