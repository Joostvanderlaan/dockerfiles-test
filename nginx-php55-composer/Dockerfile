FROM ubuntu
MAINTAINER Eugene Ware <eugene@noblesamurai.com>
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y upgrade

# Keep upstart from complaining
RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl

# Otherwise you cannot add repositories
RUN apt-get install -y software-properties-common python-software-properties

# add php5.5 repository
RUN add-apt-repository -y ppa:nginx/stable
RUN add-apt-repository -y ppa:ondrej/php5 > /dev/null 2>&1 # dirty fix for "invalid UTF-8 in string"
RUN apt-get update -y

# Basic Requirements
RUN apt-get -y install nginx php5-fpm php5-mysql  pwgen python-setuptools curl git unzip
#mysql-server mysql-client php-apc >> opcache is included in php5

# Wordpress Requirements
RUN apt-get -y install php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

# mysql config
#RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

# Laravel requirements
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/bin/composer

# nginx config
RUN sed -i -e"s/keepalive_timeout\s*65/keepalive_timeout 2/" /etc/nginx/nginx.conf
RUN sed -i -e"s/keepalive_timeout 2/keepalive_timeout 2;\n\tclient_max_body_size 100m/" /etc/nginx/nginx.conf
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# php-fpm config
RUN sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" /etc/php5/fpm/php.ini
RUN sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" /etc/php5/fpm/php.ini
RUN sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" /etc/php5/fpm/php.ini
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php5/fpm/php-fpm.conf
# Joost
#ADD ./opcache.ini /etc/php5/cli/conf.d/
###ADD ./opcache.ini /etc/php5/fpm/conf.d/opcache.ini
# Joost end
RUN find /etc/php5/cli/conf.d/ -name "*.ini" -exec sed -i -re 's/^(\s*)#(.*)/\1;\2/g' {} \;

# For newer NGINX
#RUN mkdir /var/www/vhosts/www
# nginx site conf
#ADD ./nginx-site.conf /etc/nginx/conf.d/
####ADD ./nginx-site.conf /etc/nginx/sites-enabled/default

# Supervisor Config
RUN /usr/bin/easy_install supervisor
ADD ./supervisord.conf /etc/supervisord.conf

#The trailing / in /scripts/ tells Docker to treat it as a folder else without that /scripts will be treated as a file.
# The <src> path must be inside the context of the build; you cannot ADD ../something /something
####ADD ./ /var/www/vhosts/laravel/
RUN chown -R www-data:www-data /var/www/vhosts
####RUN chown -R www-data:www-data /var/www/vhosts/laravel

# fix the services.json error when doing composer install
####RUN chmod -Rvc 777 /var/www/vhosts/laravel/app/storage
####RUN composer --working-dir=/var/www/vhosts/laravel install --prefer-dist

# Phpinfo for testing purposes
RUN echo "<?php phpinfo(); ?>" > /var/www/vhosts/laravel/public/phpinfo.php

# Wordpress Initialization and Startup Script
ADD ./start.sh /start.sh
RUN chmod 755 /start.sh

# private expose
EXPOSE 80

CMD ["/bin/bash", "/start.sh"]
