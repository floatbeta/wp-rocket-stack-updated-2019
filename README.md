# WP Rocket Stack updated for PHP7.4 FPM
I forked this to make it compatible with PHP 7.4.

### We are going to show you how to use this

1. You need root/ssh access to your server. IF YOU DO NOT HAVE THIS IT WILL NOT WORK!

2. This works with Ubuntu 16.04 server to Ubuntu 20.04 server.

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
#### cd install
#### wget https://dev.mysql.com/get/mysql-apt-config_0.8.12-1_all.deb
#### dpkg -i mysql-apt-config_0.8.12-1_all.deb
#### apt update
#### apt upgrade
#### apt install mysql-server
#### apt install software-properties-common
#### add-apt-repository ppa:ondrej/php
#### apt update
#### apt install php7.4
#### apt purge apache2
#### apt install nginx
#### apt install tmux curl php7.4-fpm php7.4-curl php7.4-intl php7.4-cli php7.4-mysql php7.4-gd php7.4-imagick php7.4-tidy php7.4-xmlrpc php7.4-mbstring php7.4-zip php7.4-xml unzip

#### apt install redis-server
#### apt install install fail2ban

## Configuring Redis to be a non-persistent cache

You don’t want Redis writing to disk – we’re just using it as an object-cache and variant-cache, and anything using the object-cache of variant-cache will survive the cache being wiped (it’ll just start building the cache again) so you need to alter the config to avoid disk writes which would otherwise slow your server down.

Edit /etc/redis/redis.conf and add the following 2 lines at the end:

#### nano /etc/redis/redis.conf

Scroll to the bottom of the file.
Make adjustments to the value, example: VPS w/1GB of Ram insert maxmemory 100mb, 2gb of Ram insert 200mb...

#### maxmemory 100mb

Now add this line below the other one you just added. This forces old keys to be deleted using first-in-first-out.

#### maxmemory-policy allkeys-lru

Now locate these 3 lines and comment them out by adding an astrick in front of them.
Commenting out these 3 lines prevents Redis from writing anything to disk, giving you a true in-memory cache.

#### #save 900 1
#### #save 300 10
#### #save 60 10000

Now save it and exit out of nano, and restart Redis.

#### service redis-server restart

## Configuring Nginx to serve your website in the fastest possible way

The config files I use are built to allow processes to run for ages – this helps if you’re running massive import or export jobs etc – but you can modify them using the comments included in the config files.

#### cd ~
#### git clone https://github.com/floatbeta/wp-rocket-stack-updated-2019
#### cp wp-rocket-stack-updated-2019/nginx/* /etc/nginx/ -R
#### ln -s /etc/nginx/sites-available/rocketstack.conf /etc/nginx/sites-enabled/
#### rm /etc/nginx/sites-enabled/default

The files you cloned above and copied to your nginx folder include a config file for your website as well as various snippets to make your site fast and secure. The files use the nginx_fastcgi_cache library, and for that to work you need to create a cache folder.

#### mkdir /var/www/cache
#### mkdir /var/www/cache/rocketstack
#### chown www-data:www-data /var/www/cache/ -R

Before restarting nginx, you should alter the rocketstack.conf file – specifically, you want to enter your own domain name to prevent botnets attacking your site via the IP address.

#### nano /etc/nginx/sites-available/rocketstack.conf

Then change the server_name _; line to read:

#### server_name www.yourdomain.com;

To get these files into your nginx installation, you’ll need to restart nginx using the following command:

#### service nginx restart

Now your web server is ready for traffic, so visit www.yourdomain.com in your browser and check that you see 404 error, that confirms that Nginx is working.

Now just setup the database and install Wordpress!

To continue this guide please refer to https://www.wpintense.com/2018/10/20/installing-the-fastest-wordpress-stack-ubuntu-18-mysql-8/

