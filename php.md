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

## Composer setup
1. go to https://getcomposer.org/ and click download
2. click "Composer-Setup.exe" from Window Installer; then open the .exe and click "Install for all users"
3. click Next(for Installation option) -> choose the php ver. you want -> click Next(for Settings check) -> click Next(for Proxy settings) -> take a look and click "Install" -> click Next(for Information) -> click Finish
4. open cmd and type composer to check

## Drupal setup
1. before install Drupal, you've to install git, php & composer(please check PHP and Composer setup guide shown above)
2. go to cmd of desktop or the directory you wanna build your Drupal project and type the command below
```
composer create-project drupal/recommended-project <<project name>>
```

## PHP setup(laragon)
1. Go to https://laragon.org/ , click Download, then click "Download Laragon - Full (147 MB)"
2. open the .exe, choose English, choose the directory and press next, and press next again, then click Install
3. open Laragon Menu, press the Preference logo on top right corner, you can see your document root, that storing all your .php file later on; usually put in "C:\laragon\www"
4. when you press "Start All", it'll automatically starts the php server and MySql, you can type 'localhost' or 'http://localhost/index.php' on URL to test it
5. you can then open some folder in www folder(mentioned in step 3), and open them in VS code, you can open a index.php(similar to html file) in that folder if you like; then you can open it in local host by typing "http://localhost/<< folder name>>>/index.php"


## Laravel Set up
- if larave彈error like "could not be opened: failed to open stream: Permission denied", 要用chmod 777 to solve