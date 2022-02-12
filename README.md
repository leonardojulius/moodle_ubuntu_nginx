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

  
