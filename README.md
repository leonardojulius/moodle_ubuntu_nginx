<h1><p align="center">
Install Moodle LMS on Ubuntu 20 using (NGINX)
</p></h1>

<p align="center">
  <img src="https://moodle.org/theme/image.php/moodleorg/theme_moodleorg/1642682278/moodle_logo_small" alt="Sublime's custom image"/>
</p>

Source from [https://fedingo.com/](https://computingforgeeks.com/install-node-js-14-on-ubuntu-debian-linux/).

Layout Guide [Github-Formatting](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

--Arrange By Julius Leonardo


<h3><p align="left">
How to Install Moodle with NGINX on Ubuntu <br>
</p></h3>

<h5><p>
Here are the steps to install Moodle with NGINX on Ubuntu.
</p><h5>

### 1. Install NGINX
  <p>Open terminal and run the following commands to update system and install NGINX.</p>

```
$ sudo apt-get update
$ sudo apt-get install nginx
```
  <p>Run the following commands to enable NGINX to autostart during reboot, and also start it now.</p>
  
```
$ sudo systemctl stop nginx.service 
$ sudo systemctl start nginx.service 
$ sudo systemctl enable nginx.service
```
  
### 2. Install MariaDB/MySQL
  <p>Run the following commands to install MariaDB database for Moode. You may also use MySQL instead.</p>

```
$ sudo apt-get install mariadb-server mariadb-client
```  
  <p>Like NGINX, we will run the following commands to enable MariaDB to autostart during reboot, and also start now.</p>
  
```
$ sudo systemctl stop mysql.service 
$ sudo systemctl start mysql.service 
$ sudo systemctl enable mysql.service
```
  <p>Run the following command to secure MariaDB installation.</p>
  
```
$ sudo mysql_secure_installation
```  
  <p>You will see the following prompts asking to allow/disallow different type of logins. Enter Y as shown.</p>

```
 Enter current password for root (enter for none): Just press the Enter
 Set root password? [Y/n]: Y
 New password: Enter password
 Re-enter new password: Repeat password
 Remove anonymous users? [Y/n]: Y
 Disallow root login remotely? [Y/n]: Y
 Remove test database and access to it? [Y/n]:  Y
 Reload privilege tables now? [Y/n]:  Y
```
  <p>After you enter response for these questions, your MariaDB installation will be secured.</p>
  
### 3. Install PHP-FPM & Related modules
<p>Run the following command to add PHP repository, install it along with other related modules.  </p>

```
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt update
$ sudo apt install php7.1-fpm php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-gd php7.1-xml php7.1-intl php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-ldap php7.1-zip php7.1-curl
```
  
  <p>Open PHP-FPM config file.</p>
  
  ```
  sudo vi /etc/php/7.1/fpm/php.ini
  
  ```
  <p>Add/Update the values as shown. You may change it as per your requirement.</p>
  
```
file_uploads = On 
allow_url_fopen = On 
memory_limit = 256M 
upload_max_filesize = 64M 
max_execution_time = 360 
cgi.fix_pathinfo = 0 
date.timezone = America/Chicago
```
  
### 4. Create Moodle Database
  <p>Log into MySQL and create database for Moodle.</p>

  ```
$ sudo mysql -u root -p
```

<p>Create a database for moodle, a database user to access it, and also grant full access to this user. Replace moodle_user and moodle_password with your choice of username and password.</p>
  
```
CREATE DATABASE moodle;
CREATE USER 'moodle_user'@'localhost' IDENTIFIED BY 'moodle_password';
GRANT ALL ON moodle.* TO 'moodle_user'@'localhost' IDENTIFIED BY 'moodle_password' WITH GRANT OPTION;
```
  <p>Flush privileges to apply changes.</p>

```
FLUSH PRIVILEGES; 
EXIT;
```

### 5. Download & Install Moodle
 <p>Run the following command to download Moodle package.</p>
  
```
$ cd /tmp && wget https://download.moodle.org/download.php/direct/stable33/moodle-latest-33.tgz
 
```
   <p>Run the following command to extract package to NGINX website root folder.</p>
 
```  
$ sudo tar -zxvf moodle-latest-33.tgz 
$ sudo mv moodle /var/www/html/moodle 
$ sudo mkdir /var/www/html/moodledata
```
  <p>Change the folder permissions.</p>

```
$ sudo chown -R www-data:www-data /var/www/html/moodle/ 
$ sudo chmod -R 755 /var/www/html/moodle/ 
$ sudo chown www-data /var/www/html/moodledata
```
### 6. Configure NGINX
<p>Now we will configure NGINX to serve files from moodle folder. For that, we will create a virtual host configuration file in NGINX.</p>

```
$ sudo vi /etc/nginx/sites-available/moodle.conf
```  
