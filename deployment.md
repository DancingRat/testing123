# Docker
## installing WSL in Window
1. right click powershell and press 以系統管理員身份執行
2. run below command and restart the computer afterward
```
wsl --install
```
3. after restart, you've to wait for a few mins for installing ubuntu; then you can set you username and password afterward

## installing Docker
- remark: you can only install Docker in Window 10; it can't be installed in Window 7 or 8
- Docker command

| Docker Commands  | Description  |
|---|---|
|docker ps   |List all of the running container   |
|docker ps -a   |List all of the container, including the stopped one   |
|docker rm _container_id_   |Remove container by id   |
|docker images   |Show all docker images   |
|docker rmi _image_tag_   |Remove docker image by tag   |
|docker logs _container_id_   |Show logs of container by id   |
|docker run _OPTION_ _image_tag_  |Start container from the docker image   |
|docker build -t _tag_name_ .   |Build a docker image according to the file called Dockerfile in the current directory   |


1. download it from https://docs.docker.com/desktop/windows/install/ ; then it will ask you to lockout/restart your computer
2. you can register and sign in docker hub by going https://hub.docker.com/
3. 可喺一個directory(in server folder usually, if the docker is for backend)起一個file叫Dockerfile, 然後入去個directory求其放啲code入去like below
```docker
FROM node:16
WORKDIR /usr/src/app
COPY . .
EXPOSE 8080
RUN npm install -g yarn && \ 
    yarn install
```
```
再run
docker build -t docker-example .
去建立一個叫docker-example嘅tag(docker image)

然後你可run this image by the below command
docker run -d -it -p 8080:8080 docker-example 
P.S.: -d stand for run the container in detached mode (in the background);
-it stands for keeping the interactive mode and TTY mode(Text telephone);
-p 8080:8080 stand for map port 8080 of the host to port 8080 in the container
```
- 以上示範用到FROM, RUN, EXPOSE等字眼, 其實都是docker嘅keyword using in docker file, below is a list of docker keyword

| Docker Keywords  | Usage  |
|---|---|
|FROM   |Specify the base image of the current image to base on   |
|RUN   |You can write in two formats: RUN _command_ or RUN ["executable","param1","param2"] to run any commands you want.   |
|EXPOSE   |Specify what port is exposed for your container.   |
|CMD   |It is similar to RUN in format. But the difference is that RUN specifies the build steps while CMD specifies the default command when the container is started.   |
|ENV   |Set the ENV for this container. You can think of this one as .env.   |
|WORKDIR   |WORKDIR specifies the working directory for other commands. Hence it is normally one of the first command to be run.   |

4. build a .dockerignore file in the same directory as Dockerfile, it should include some files/folders you don't wanna copy
```
e.g.
node_modules
<<photo-containing-folder>>
.env
.env.example
<<volume-created-folder from compose file>>
```
5. build file call docker-compose.yml, below is an example for node as a server and psql as a DB

```yml
version: '3'
services:
  server:
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: c19_frd_revision
      POSTGRES_HOST: postgres
      NODE_ENV: production
      PORT: 8080
    depends_on:
      - postgres
    build:
      context: ./
      dockerfile: ./Dockerfile
    image: "c19-frd/revision-pokemon:latest"
    ports:
      - "8080:8080"
  postgres:
    image: "postgres:13"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: c19_frd_revision
    ports:
      - "5432:5432"
    volumes:
      - ./pgdata:/var/lib/postgresql/data

```
- for MongoDB, it should be like
```yml
version: '3.8'
services:
  mongo_db:
    image: "mongo:4.0"
    ports:
      - "27017:27017"
    volumes:
      - ./mongodata:/data/db
  api:
    environment:
      MONGODB_URL: mongodb://mongo_db:27017
    build: 
      context: ./
      dockerfile: ./Dockerfile
    image: "c19-frd-006-image:latest"
    ports:
      - "8080:8080"
    depends_on:
      - mongo_db
volumes:
  mongo_db:
```
6. then you can run the command below, to run your docker-compose.yml(including installing all the packages), then build container accordingly
```
docker-compose up
```
P.S.: beware that stuff inside your devDependencies in package.json will not be install, if you wanna install, you've to move them to dependencies in package.json
- you can run the command below if you wanna remove your container
```
docker-compose down
```
P.S.: docker-compose down only remove container, for the images, you've to run 
```
docker rmi <<container_id>>
```
- if you wanna enter the container 去做野, 就可打
```
docker exec -it <<container name>> bash
想走就打返
exit
```
7. After docker-compose up, the server and DB should be running, you can use Insomnia to test the node server, and enter psql container(with psql -U command) to test the DB


## AWS
1. register and sign in through https://aws.amazon.com/
2. remember that you've to set your AWS region to 'Singapore' at the top right corner
3. go to 'Launch an instance'
```
for name: it's 9改
for quick_start: choose ubuntu
for Amazon Machine Image (AMI): choose some AMI that is 'Free tier eligible' if possible
for Instance type:choose 'Free tier eligible' if possible
for key pair name:choose one(it should be in your ls ~/.ssh) or create a new one(you should create a new key for each new project; and the type should be rsa and the file format should be .pem)
Configure storage:make root volume bigger like 1 X 30 Gib gp2
Termination protection:Enable

then you can launch Instance
```
4. go back to instance and press 'F5', then click the newly created instance then click Security then click it on Security groups
5. click Action and Edit inbound rules; then add 2 rules that the type should be set as HTTP and HTTPS, and the CIDR block should be 0.0.0.0/0 for that 2 rules; then press 'Save rules'
6. click Elastic IPs at the side bar, then click Allocate Elastic IP address; normally nothing there should be change so you can just press Allocate, then give a name to that Elastic IP addresses
7. Click Action and Associate Elastic IP address, choose the instance as the one you just created, then click Associate
8. go back to instance and copy the Elastic IP; then go to :C/Users/user/.ssh/config
```
The IdentityFile should be ~/.ssh/<<your key pair name>>.pem
and the Host name should be <<your copied elastic IP>>, so the .ssh should like below

Host <<9改>>
  Hostname <<your copied elastic IP>>
  User ubuntu
  IdentityFile ~/.ssh/<<your key pair name>>.pem
```
9. then you can type "ssh _your 9改 Host名 from above_" in your terminal to enter the ubuntu; then you can run the command below
```
- sudo apt-get update
- sudo apt-get upgrade (be careful for this upgrade command as sometimes you wanna keep the older version)
- sudo apt-get install build-essential nginx htop git ranger (beware that some package you may not need to install)
- sudo apt-get remove docker docker-engine docker.io containerd runc (only run this if you wanna delete the old version of docker)
- sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor-o /usr/share/keyrings/docker-archive-keyring.gpg
- echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
- sudo apt-get update
- sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
- sudo groupadd docker
- sudo usermod -aG docker $USER
```
then you can exit the ubuntu and re-enter, then run docker --version, and docker compose --version, and docker ps -a, and docker images to check
- if you're using docker to deploy
  1. type the following in your terminal in server folder(not ubuntu terminal)
  ```
  docker save <<your targeted image>>:latest | gzip > <<9改一個名of that image for tar.gz format>>.tar.gz(it may not work in Window)

  For Window,
  docker save -o <<9改一個名of that image for tar format>>.tar <<your targeted image>>
  ```
  2. you need to move the .tar file and docker compose file from local to remote host, so type the below in your terminal in server folder
  ```
  scp ./<<.tar file you've just created>>.tar ubuntu@<<上面你在.ssh入面的config file裡9改的Host名(for the specific AWS Elastic IP)>>:~

  scp ./docker-compose.yml ubuntu@<<上面你在.ssh入面的config file裡9改的Host名(for the specific AWS Elastic IP)>>:~
  ```
  then you can enter back to ubuntu terminal(by ssh _your 9改 Host名 from above_), then type "ls" to check whether the file is there or not
  3. In ubuntu terminal, type below command to load the image into ubuntu
  ```
  docker load < <<.tar or .tar.gz file you created above>>
  ```
  then you can type "docker images" to check; and type "docker compose up" to run
