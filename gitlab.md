# gitlab guide
## git installation(when your computer haven't done it yet)
1. Go to https://git-scm.com/ and click "Download for Windows"(click Mac if you're using Mac)
2. After installation, you can type "git --version" in powershell(or in VS code) to check is it installed
3. You then have to set up "git config user.name" and "git config user.email" by the below command
```
- git config --global user.name "<<9改a name for you>>"
- git config --global user.email "<<9改a email for you>>"

P.S. You can set multiple user & email in one computer
```
then you can check the user.name and user.email that you've set by the following command
```
- git config user.name
- git config user.email
```
4. testing checkout

## **gitlab start up procedure**
1. login your gitlab
2. you've to generate a new SSH key if you don't have one
```
1. run the following command, It will prompt you for some information , just keep on pressing Enter to use the default value.
ssh-keygen
2. then it'll generate a public key & private key, run the following command, to get the public key
cat ~\.ssh\id_rsa.pub
or,
cat ~\.ssh\id_dsa.pub
you can also type ls ~/.ssh/*.pub to see how many keys do you've
```
3. copy the public key generated from the step 2, and put it into gitlab
```
位置
gitlab main page --> press icon button from top right corner -->  press 'preference' --> press 'SSH keys' in left side bar --> paste the public key in key's textarea
```
4. back to gitlab main page and press 'New project', then press 'Create blank project' normally 
5. 1 fill in the form(for 'Create blank project')
```
- name your project
- for the 'Project URL', choose your Username if you've no idea
- Visibility Level should be 'private' normally
- and Initialize repository with a README if you want
```
6. 1 after enter your newly created project, press 'clone', then Copy URL from 'Clone with SSH'
7. 1 after copied the URL, run the following command in your targeted directory(can be desktop, or a specific file you like)
```
git clone <<Copied URL from step 6.1>>
```


## **git command**
- git --version
    - to check the version of git
- git config --global core.editor "<u>nano</u>"
- git config --global user.name "<u>John Doe</u>"
- git config --global user.email "<u>john.doe@gmail.com</u>"
    - config your git in your PC, username and email are compulsory
- git init
    - Initialize an empty Git repository; your can check the existence of your git repository by running 'ls -Force'
- git status
    - check your git status
- git log
    - can check your previous committed version, with your config name and email; your can also check the detail about this from Git Graph(extension in VS code)
- git restore --staged <u>filename</u>
    - to restore a staged file, which mean making a staged file becoming untracked
- git checkout <u>committed message</u>
    - to rollback to your previous version; your can use git log/Git Graph to find the version that your're looking for
---
- git add <u>filename</u>
    - can make a file tracked/staged
- git add .
    - can make all files in current folder/directory tracked/staged
- git commit -m "<u>remark message of this version</u>"
    - confirm the version of the git repository folder, including all staged files
- git push
    - after your git repository has linked to your project in Gitlab/GitHub, and after git commit, your can push your committed version to your project and let your teammates/guest know
- git pull
    - the opposite of git push, which pull the latest committed version from your project in Gitlab/GitHub