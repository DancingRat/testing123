# Linux
## To rename a folder in Linux 
- mv << old folder >>/ <<新名of the folder>>/

## Move folder from remote to remote
- scp -r [file/dirname] [server user]@[host]:[path]
## Set up Mysql in new VM
1. sudo -i to enter root and, mysql -u root -p, password is nothing
2. CREATE USER '<< username >>'@localhost IDENTIFIED BY '<< password >>';
3. create database << DB name >>;
4. GRANT ALL PRIVILEGES ON << DB name >>.* TO << username >>@localhost;

- command of entering mysql: mysql -u [db username] -p

## Import & Export mysql dump file
- Export
  - mysqldump -u [username] -p [database] > [想改的.sql filename]
- Import
  - sudo mysql -u root -p [database] < [想改的.sql filename].sql


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
|docker rmi _image_id_   |Remove docker image by id   |
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
docker-compose.yml
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
or run
docker-compose up -d
to run it at background
```
P.S.: beware that stuff inside your devDependencies in package.json will not be install, if you wanna install, you've to move them to dependencies in package.json
- you can run the command below if you wanna remove your container
```
docker-compose down
```
P.S.: docker-compose down only remove container, for the images, you've to run 
```
docker rmi <<container_id>>
- if you wanna remove all container then remove all images all in one go
要先
docker container rm -f $(docker container ls -ap)
再remove images
docker image rm -f $(docker image ls -ap)
- 或者你可開docker app
press 'trouble shoot' button(蟲仔icon), then press 'Clean / Purge data'
  - if you're using this method, beside of delete data, it'll also restart docker automatically
```
- you can run the command below to check the current container running
```
docker-compose ps
```
- if you wanna enter the container 去做野, 就可打
```
docker exec -it <<container name>> bash
或者
docker run -it <<docker image tag>>
想走就打返
exit
如想要最高權限就打
docker exec -it -u root <<container name>> bash
```
7. After docker-compose up, the server and DB should be running, you can use Insomnia to test the node server, and enter psql container(with psql -U command) to test the DB


## AWS
1. register and sign in through https://aws.amazon.com/
2. remember that you've to set your AWS region to 'Singapore' at the top right corner
3. go to 'Launch an instance' (EC2)
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
  docker load < <<.tar or .tar.gz file you created above>>.tar
  ```
  then you can type "docker images" to check; and type "docker compose up" or "docker-compose up -d" to run
  
  4. after the docker setup, it's time for nginx part, type below command
  ```
  - sudo service nginx start

  - sudo nano /etc/nginx/sites-available/default

  - then put the below words inside location /{ }

  proxy_pass http://localhost:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;

  remember to comment the "try_files $uri $uri/ =404" inside location /{ }
  - then press "Ctrl S" to save and "Ctrl X" to exit

  - sudo service nginx restart
  ```
  then you can put your Elastic IP in your browser then you will discover that the nginx site is connecting our docker server in our ubuntu



# Deploying React separately
## yarn build
1. put the code below into the .env file inside my-app(react folder), then run yarn build
```
GENERATE_SOURCEMAP=false
```
2. go to AWS main page, click top right corner and click Security credentials
3. click Users at sidebar then click Add users, then fill in the form by the following steps
```
1. - the User name is 9改
   - tick the checkbox of "Access key - Programmatic access", then click Next: Permissions
2. - can keep the default setting, and click Next:Tags
3. - can keep the default setting, and click Next:Review
4. - can keep the default setting, and click Create user
5. - then you can download the csv
```
4. run the command below in terminal inside my-app(react folder)
```
pip install awscli

aws configure

# Copy AWS Access Key ID & Secret from rootkey.csv(the csv file downloaded from step 3.5)
AWS Access Key ID [********************]: <<Access Key ID from csv>
AWS Secret Access Key [********************]: <<Secret Access Key from csv>>

# Most of the case in our course, we use Singapore as AWS location and the region of it is "ap-southeast-1"
Default region name [ap-southeast-1]: ap-southeast-1
Default output format [json]: json
```
5. type "s3" in the search bar of AWS, then click S3, then click Create bucket, and fill in the the form following steps below
```
- Bucket name: can 9改, but bear in mind that this name should be unique(意味這name不可與世上任一個Bucket撞名)
- AWS region: should be Asia Pacific(Singapore) ap-southeast-1
- untick the checkbox "Block all public access"
  - and tick the relative checkbox "I acknowledge that the current settings might result in this bucket and the objects within becoming public."
- then should keep all other setting as default and click Create bucket
```
6. click bucket at sidebar and click your newly created bucket then click Properties, scroll down to "Static website hosting" and click Edit, and fill in the form like below
```
- tick the 'enable' checkbox
- then type "index.html" in the Index document
- then should keep all other setting as default and click Save changes
```
7. back to your bucket and click "Permission", then click Edit on Bucket policy, then you should edit the Policy like below, then click Save changes
```
{
	"Version": "<<use the default version it provided>>",
	"Statement": [
		{
			"Sid": "Statement1",
			"Principal": "*",
			"Effect": "Allow",
			"Action": "s3:GetObject",
			"Resource": "<<the Amazon Resource Name (ARN) that you can copy from your bucket --> Properties>>/*"
		}
	]
}
```
8. click top right corner and click Security credentials, then click Users, and click the user you created from step 3, then click "Add inline policy" from the Permission part, then click JSON and edit it like below, then click Review policy
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1546023631000",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListObjects",
                "s3:ListBucket",
                "s3:PutObject"
            ],
            "Resource": [
                "<<Amazon Resource Name (ARN)>>",
                "<<Amazon Resource Name (ARN)>>/*"
            ]
        }
    ]
}
```
In Review policy, the Name is 9改, then you can click Create policy

9. back to terminal inside the build folder(the folder you just yarn build at step 1), the type the command below
```
aws s3 sync . s3://<<your newly created bucket name>>
```
then when you back to your bucket in S3 page, you can press F5 then click Object and you will see those files&folders from build folder are there

10. When you click Properties from your bucket, scroll down to "Static website hosting", you'll discover that your got a url(Bucket website endpoint), which can go to your react index page

11. Go to CloudFront: https://console.aws.amazon.com/cloudfront , then click Create a CloudFront distribution; scroll down to "Custom SSL certificate - optional" then click Request certificate, then you'll discover that you're in Request public certificate page and the region in top right corner changed to "N. Virginia" which is __correct__

12. In Request public certificate page, fill in the form like below
```
- Fully qualified domain name: <<your purchased domain name>>
- Select validation method: better select DNS validation
- then should keep all other setting as default and click Request
```
after that, you can press F5 on you Certificates page, and you'll discover your certificate on the list

13. Click in your Certificate, and click "Create records in Route 53" 
  - make sure you've created the hosted zone in Route53 before you do this step; for creating hosted zone, please see "DNS & Route53 setup on AWS and NameCheap" in below section

14. select your Domain and press "Create records", then you'll discover that it will appear in your Hosted zone in Route53, and the Type will be "CNAME"; and your Certificate status will be "Success" after a moment and press F5

15. back to the create form of CloudFront distribution(in step 11) and press F5, then fill in the form like below
```
- Origin domain: <<the Bucket website endpoint(from your S3 Bucket -> Properties -> Scroll down to Static website hosting)>>
- Viewer protocol policy: tick the checkbox "Redirect HTTP to HTTPS"
- Alternate domain name (CNAME) - optional: press "Add item", then put your domain name inside(也是你Certificate CNAME 的尾段部份)
- Custom SSL certificate - optional: <<the certificate you've just created from above steps>>
- Default root object - optional: index.html
- then should keep all other setting as default and click Create distribution
```

16. click top right corner and click Security credentials and click Users at sidebar, then click the user you've created at step 3, and click Add inline policy and click JSON, then edit it like below, then click Review policy
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1546023707000",
            "Effect": "Allow",
            "Action": [
                "cloudfront:CreateInvalidation"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```
In Review policy, the Name is 9改, then you can click Create policy

17. back to your hosted zone of Route53, click Create record, then make sure the "Record type" is "A", and keep the Record name blank if wanna create record for the root domain. Then, turn on the Alias and choose Alias to CloudFront distribution, and Choose distribution(should be Distribution domain name in your CloudFront distribution), then click Create records
  - after that, you'll discover that the "Record name" you just typed in this step can put in your browser and reach to your front-end

18. In order to connect your docker you've to create a new record in Route53 just like step 17; but the only difference are the 
  - "Record name" can't keep it blank and you should type in something that let you know it's connecting to the docker e.g. "server"
  - and the "Value" should be << Your Elastic IP created above which put in .ssh config file>>
after that, you'll discover that the "Record name" you just typed in this step can put in your browser and reach to your back-end(created by docker)

19. Therefore, in the .env file in your my-app(react app) folder, you should change the react api from REACT_APP_API = http://localhost:8080/api to https://<< The Record name you created from step 18>>/api
  - beware that it changed from http to https

20. To setup HTTPS, you've to re-enter nginx by "sudo nano /etc/nginx/sites-available/default", then there should be a "server_name _;" right above location /{ } , you should make the server_name _; like below
  ```
  server_name << The Record name you created from step 18>>;

  then press "Ctrl S" to save then exit and run the command below in ubuntu
  
  sudo apt-get update
  sudo apt-get install software-properties-common
  sudo add-apt-repository universe
  sudo apt-get update
  sudo apt-get install python3-certbot-nginx 

  sudo certbot --nginx
  - during configuring cerbot, it will ask you serval questions, and you should answer like this
    - Enter email address(used for urgent renewal..): 9入一個你會記得嘅email
    - (A)gree/(C)ancel: a
    - (Y)es/(N)o: n
    - ...blank to select all option shown (Enter 'c' to cancel): just press enter
    - Select the appropriate number [1-2] then [Enter](Press 'c' to cancel): 2

  sudo service nginx restart

  sudo crontab -e
  - during this process, it may ask you a question, and you should answer like this
    - Choose 1-4[1]: 2
  - then you should see something like Linux command prompt, right click once, then paste the below code at the bottom, right under "# m h  dom mon dow   command"
    - 0 0,12 * * * sleep _random_0_to_3600_ && certbot renew -q && service nginx restart
    - for this Linux command prompt, press i to enable inserting/pasting/typing function; then press ESC to exit insert; then press :wq and Enter to exit
  ```

21. you may need to create and connect another bucket for your photos, so you've to repeat step 5 if you wanna do so
  - if you don't need to, just jump to step 25
22. after that, click your newly created bucket(for images) then click "Objects", and click "Upload", then drag all the chosen photos inside, then click upload
23. Then you've to edit on Bucket policy like step 7, and you can also see below
```
{
	"Version": "<<use the default version it provided>>",
	"Statement": [
		{
			"Sid": "Statement1",
			"Principal": "*",
			"Effect": "Allow",
			"Action": "s3:GetObject",
			"Resource": "<<the Amazon Resource Name (ARN) that you can copy from your bucket --> Properties>>/*"
		}
	]
}
```

24. When click on those photos in bucket, then click properties, you'll discover the Object URL which you can copy&paste on browser and see the image; therefore, in the .env file in your my-app(react app) folder, you should change the react api from REACT_APP_IMAGE_URL = http://localhost:8080/api to << The front bit of Object URL>>

25. As step 24 & 19 have changed the content of .env, so we've to do delete the build folder in our my-app(react app folder) and run yarn build again; then run step 9 again

26. Go back to CloudFront: https://console.aws.amazon.com/cloudfront, enter your newly created cloudfront, click "Error pages" and click "Create error custom response", and fill in the form like below, then click "Create error custom response"
```
- HTTP error code: select "400: Bad Request"
- Customize error response: tick yes on checkbox
  - Response page path: "/index.html"
- HTTP Response code: select "200: OK"
```
- beside of 400, we also have to create another error response for 404, and the "Create error custom response" form will like below
```
- HTTP error code: select "404: Not Found"
- Customize error response: tick yes on checkbox
  - Response page path: "/index.html"
- HTTP Response code: select "200: OK"
```

27. In our CloudFront, we still have to click on Invalidations, and click "Create invalidation", then put /* inside the "Add object paths" and press "Create invalidation"
  - bear in mind that whenever there is a change on any settings, you better repeat this step again





# Domain Name
- you can watch this Tecky video for buying domain name tutorial https://www.youtube.com/watch?v=PuFQrtYOPa0&list=PLjB7odcPYBI1HhL9CKNEWHgK1boJRbUsJ&index=4

## buying domain name from NameCheap
1. go to https://www.namecheap.com/ , sign up and sign in your account, then search for your ideal domain name 
2. pick the ideal one then click "Add to cart" and "Checkout" 
3. to in the Checkout page, make sure you turn off the "AUTO-RENEW" if your website is just for fun or temporary; then other setting should remain default, and you can click Confirm Order
4. Follow the steps it guide you until the payment complete; In Purchase Summary, your can click "Manage" on your domain name, for further operation later(like DNS&Route53 setup)

## DNS & Route53 setup on AWS and NameCheap
1. sign in AWS and type Route53 in search bar and enter Route 53 Dashboard, then click "Created hosted zone"
2. In Created hosted zone, put your newly purchased domain name in "Domain name", and make sure your tick the "Public hosted zone", then you can click "Created hosted zone"
3. In Hosted zone of your domain name, copy all the "Value/Route traffic to"(only the Type NS)
4. back to your NameCheap -> Account -> Dashboard -> Domain List, then click "Manage" of your targeted domain name, then select "Custom DNS" on "NAMESERVERS", then paste all copied from step 3 there separately, then click the tick icon button to save changes
  - can click "ADD NAMESERVER" on NameCheap if there was not enough space
  - you've to wait for hours to make this 生效, usually less than 48hours
