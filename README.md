<h1><p align="center">
Install Moodle LMS on Ubuntu 20 using (NGINX)
</p></h1>

<p align="center">
  <img src="https://moodle.org/theme/image.php/moodleorg/theme_moodleorg/1642682278/moodle_logo_small" alt="Sublime's custom image"/>
</p>

Source from [https://fedingo.com/](https://fedingo.com/how-to-install-moodle-with-nginx-on-ubuntu/).

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
$ sudo vi /etc/nginx/sites-available/moodle
```  
<p>Copy paste the following configuration in the above file. Replace example.com below with your domain name.</p>

```
server {
    listen 80;
    listen [::]:80;
    root /var/www/html/moodle;
    index  index.php index.html index.htm;
    server_name  example.com www.example.com;

    location / {
    try_files $uri $uri/ =404;        
    }
 
    location /dataroot/ {
    internal;
    alias /var/www/html/moodledata/;
    }

    location ~ [^/]\.php(/|$) {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

}
```

  <p>Enable Moodle site in NGINX by creating a symlink as shown below.</p>

```
$ sudo ln -s /etc/nginx/sites-available/moodle /etc/nginx/sites-enabled/
```

  <p>Restart NGINX Server to apply changes.</p>

```
$ sudo service nginx restart
```
### 7. Configure Moodle

<p>Open browser and go to your website (e.g. http://example.com) hosting your Moodle site. You will see the following installation screeen.</p>

![image](https://user-images.githubusercontent.com/16941074/153736465-ee9cef9e-1f79-4953-91e1-38e7b973c624.png)

  <p>Select English from the dropdown and click Next.</p>

  <p>On next screen, accept default values and click Next.</p>

![image](https://user-images.githubusercontent.com/16941074/153736489-ebbe845e-4a25-4508-9f79-2ef52da30ded.png)

  <p>On next screen, select MariaDB database driver from dropdown and click next.</p>

![image](https://user-images.githubusercontent.com/16941074/153736495-3a096ebf-0bad-485e-b6e7-3758f69ca44d.png)

  <p>Enter database user details that you had entered in Step 4.</p>
  
![image](https://user-images.githubusercontent.com/16941074/153736573-a7094af3-71dc-4140-9822-be8b36a48b15.png)

  <p>On next screen, enter username admin and enter password of your choice and click Next.</p>
  
![image](https://user-images.githubusercontent.com/16941074/153736584-ca21d751-b519-4559-8928-e3af19cb0d20.png)

  <p>Continue the wizard until you have provided the required information and set up the site. When it is complete, Moodle will be installed & ready to use.</p>
  
![image](https://user-images.githubusercontent.com/16941074/153736590-cdc6e8a9-f34e-4269-92ed-953e1989639c.png)

  <p>In this article, we have listed the various steps to install Moodle with NGINX in Ubuntu.</p>



