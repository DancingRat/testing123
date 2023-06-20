# Backend
## PHP setup
1. go to https://www.php.net/downloads.php, and click "Windows downloads" on your ideal version
2. click the "Zip" on "Thread Safe" to download the zip file, then right click the zip folder and click "extract all"
3. you can rename the unziped folder into a shorter name like "php-8.2.0" (optional)
4. cut your unziped folder to C drive; then enter the folder and copy the path(should like C:\php-8.2.7)
5. open window search, go to "Edit the system environment variables" -> click "Environment Variables" -> click "Path" -> click "Edit" -> click "New" and paste the path from step 4 and click OK
6. open command prompt, and type below command to check
```
php --version
or go the a directory that contain a php file and type
php <<filename>>.php
```
7. to prevent the "The zip extension and unzip/7z commands are both missing" error; go to php.ini and uncomment the ";extension=zip"(basically just delete the semi-colon)

## XAMPP setup
1. go to https://www.apachefriends.org/ , and download window ver.
2. keep pressing Next and download
3. then start Apache and Mysql server, then search localhost:80 on web to check

## Composer setup
1. go to https://getcomposer.org/ and click download
2. click "Composer-Setup.exe" from Window Installer; then open the .exe and click "Install for all users"
3. click Next(for Installation option) -> choose the php ver. you want -> click Next(for Settings check) -> click Next(for Proxy settings) -> take a look and click "Install" -> click Next(for Information) -> click Finish
4. open cmd and type composer to check

## Drupal setup
- setup with command
1. before install Drupal, you've to install git, php & composer(please check PHP and Composer setup guide shown above)
2. go to cmd of desktop or the directory you wanna build your Drupal project and type the command below
```
composer create-project drupal/recommended-project <<project name>>
```
- setup with zip folder(prefered when using XAMPP)
1. Go to https://www.drupal.org/download and download Drupal zip, then and unzip it
2. Go to C:\xampp\htdocs and create your Drupal project folder, then put all the files/folders into it that from the unzip folder(from step 1)
3. start the XAMPP server(Apache & Mysql), then search 'localhost/<< folder name created from step 2>>' in web
4. choose the language and choose standard for installation profile
5. then you'll get 2 errors(php gd extensions & php opcode caching);
    - for gd extensions, go to php.ini , delete the semi-colon(;) of
    ```
    ;extension=gd
    ```
    - For OPcode caching, paste the line below into the bottom of php.ini
    ```
    zend_extension = "C:\xampp\php\ext\php_opcache.dll"
    ```
    P.S. remeber to restart the XAMPP server after the step above
6. Then for Database configuration, build a new db for this project on localhost(127.0.0.1); you can use Mysql workbench or phpMyAdmin to do so. To fill in the db configuration
    - db name: << the db name you've just created>>
    - db username: should be 'root' for original local db setting
    - db pw: should be Blank for original local db setting
    - for advance option, check the host and port number; then click save
7. Finally for the configure site, customize all the stuff and click save

    P.S. For SITE MAINTENANCE ACCOUNT, this is the super admin that should have longer/complex username & pw for security purpose; and you shouldn't forget this account
8. You can install Drush by the command below
```
composer global require drush/drush:dev-master
```
9. after basic installation, there should be a vital error under Configuration, which is Trusted Host Settings Error, shown in status report; you should add the code below in C:\xampp\htdocs\<< project name >>\sites\default\settings.php
```
for local dev
$settings['trusted_host_patterns'] = ['^localhost$'];

for production, e.g.
$settings['trusted_host_patterns'] = ['^google\.com$'];

for production in sub-domain, e.g.
$settings['trusted_host_patterns'] = ['^.+\.google\.com$'];
```

## PHP setup(laragon)
1. Go to https://laragon.org/ , click Download, then click "Download Laragon - Full (147 MB)"
2. open the .exe, choose English, choose the directory and press next, and press next again, then click Install
3. open Laragon Menu, press the Preference logo on top right corner, you can see your document root, that storing all your .php file later on; usually put in "C:\laragon\www"
4. when you press "Start All", it'll automatically starts the php server and MySql, you can type 'localhost' or 'http://localhost/index.php' on URL to test it
5. you can then open some folder in www folder(mentioned in step 3), and open them in VS code, you can open a index.php(similar to html file) in that folder if you like; then you can open it in local host by typing "http://localhost/<< folder name>>>/index.php"


## Laravel Set up
- if larave彈error like "could not be opened: failed to open stream: Permission denied", 要用chmod 777 to solve