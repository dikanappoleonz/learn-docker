<!-- Heading -->
<h1 align="center">Introduction Containers</h1>

## Container vs Virtual Machine
<p align="center">
  <img src="images/images-1.png" witdh="50%" height="50%" alt="containers"/>
</p>

## Docker Architecture
<p align="center">
  <img src="images/images-2.png" witdh="50%" height="50%" alt="containers"/>
</p>

## Installation Docker
1. Setup Docker's apt repository
```js
//Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

//Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. Install the Docker packages
```js
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. Check Version Docker
```js
docker --version
```
[installation docker on Ubuntu](https://docs.docker.com/engine/install/ubuntu)

##