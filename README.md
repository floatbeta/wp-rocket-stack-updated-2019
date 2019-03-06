# WP Rocket Stack updated for PHP7.3 FPM
I forked this to make it compatible with PHP 7.3.

### We are going to show you how to use this

1. You need root/ssh access to your server. IF YOU DO NOT HAVE THIS IT WILL NOT WORK!

2. This works with Ubuntu 16.04 server to Ubuntu 18.10 server.

3. You should have at least 1 GB of ram available.

------------------------------------------------------------------------------------------------------------------

With a fresh install only!

Login to your server via SSH and get root access.

#### apt update
#### apt upgrade
#### apt install preload
#### reboot
#### cd ~
#### mkdir install
#### wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
#### dpkg -i mysql-apt-config_0.8.12-1_all.deb
#### apt update
#### apt upgrade
#### apt install mysql-server
#### apt install software-properties-common
#### add-apt-repository ppa:ondrej/php
#### apt update
#### apt install php7.3
#### apt purge apache2
#### apt install nginx
#### apt install tmux curl php7.3-fpm php7.3-curl php7.3-intl php7.3-cli php7.3-mysql php7.3-gd php7.3-imagick php7.3-recode php7.3-tidy php7.3-xmlrpc php7.3-mbstring php7.3-zip php7.3-xml unzip

#### apt install redis-server
#### apt install install fail2ban
