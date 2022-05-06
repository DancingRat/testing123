# Using GCP Compute Engine for Remote Development

# 1. Generate the New SSH keys for GCP Compute Engine

## 1.1. ssh-keygen (the username is `demo`)

```
ssh-keygen -C demo -f ~/.ssh/demo

cat ~/.ssh/demo.pub
```

## 1.2. Update ~/.ssh/config

```
Host gcp
	HostName 34.65.131.16
	User demo
	Port 22
	IdentityFile ~/.ssh/demo
```

## 1.3. Add the new SSH key (.pub) string into GCP Compute Engine

```
Compute Engine -> 

(SETTINGS) Metadata -> 

SSH KEYS -> 

[EDIT] -> 

[+ ADD ITEM] -> 

{Enter public SSH key} -> 

[SAVE]
```

# 2. Remote Development by VS Code

## 2.1. Install VS Code Extensions (Remote - SSH)

https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh

* Connect to GCP in VS Code by Remote-SSH

## 2.2. Login to Instance by VS Code (Remote - SSH)

* Click Remote Explorer (on the left side menu bar)

## 2.3. Remote Install VS Code Extensions (Docker)

https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker

* Install in SSH: gcp

## 2.4. Remote Install VS Code Extensions (MongoDB for VS Code)

https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode

* Install in SSH: gcp

## 2.5. Setup Development Environment

```
sudo apt update -y && sudo apt upgrade -y
```

## 2.5.1 Install Python 3.9

```
sudo apt install python3.9

sudo apt install python3-pip
```

## 2.5.2 Install Docker

```
sudo apt install docker.io

sudo apt install docker-compose
```

**You will have this problem:**

```
$ docker pull mongo
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.24/images/create?fromImage=mongo: dial unix /var/run/docker.sock: connect: permission denied
```

**Method 1:**
```
sudo gpasswd -a ${USER} docker

sudo reboot
```

**Method 2:**
```
sudo groupadd docker
sudo usermod -aG docker $USER

sudo reboot
```

** Please wait for few minutes, then re-login by VS Code (Remote-SSH) **

## 2.4.3. Install node.js & npm

### 2.4.3.1. Install node.js
```
sudo apt install nodejs
```

### 2.4.3.2 Install npm
```
sudo apt install npm
```

### 2.4.3.3 Update node.js & npm
```
sudo npm install -g n

sudo n latest

npm update
```

# 4. Server Rest Scheduling

Reference: https://www.youtube.com/watch?v=zZ5UfxsiwKA

# (Optional) 5. Chrome Remote Desktop

Reference: https://ubuntu.com/blog/launch-ubuntu-desktop-on-google-cloud

**Useful comand for changing default user password:**

```
sudo passwd ubuntu
```

