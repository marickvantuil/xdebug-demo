# Use phusion/baseimage as base image. To make your builds reproducible, make
# sure you lock down to a specific version, not to `latest`!
# See https://github.com/phusion/baseimage-docker/blob/master/Changelog.md for
# a list of version numbers.
FROM phusion/baseimage:0.11

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Set terminal to non-interactive
ENV DEBIAN_FRONTEND=noninteractive

# Nginx-PHP Installation
RUN apt-get update -y && apt-get install -y wget .build-essential git-core vim nano
RUN add-apt-repository -y ppa:ondrej/php && add-apt-repository -y ppa:nginx/stable
RUN apt-get update -y && apt-get upgrade -y && apt-get install -q -y php7.3 php7.3-dev php7.3-fpm php7.3-mysqlnd \
	php7.3-curl php7.3-gd php7.3-mbstring php7.3-xml php7.3-cli php7.3-intl php7.3-imap php7.3-tidy \
	php7.3-xml php7.3-xmlrpc php7.3-gmp zip unzip php7.3-zip php7.3-sqlite php7.3-imagick php7.3-bcmath php7.3-xdebug nginx-full ntp
RUN pecl install xdebug

# Create new symlink to UTC timezone for localtime
RUN unlink /etc/localtime
RUN ln -s /usr/share/zoneinfo/UTC /etc/localtime

# Update PECL channel listing
RUN pecl channel-update pecl.php.net

# Add build script
RUN mkdir -p /root/setup
ADD setup.sh /root/setup/setup.sh
RUN chmod +x /root/setup/setup.sh
RUN (cd /root/setup/; /root/setup/setup.sh)

# Copy files from repo
ADD default /etc/nginx/sites-available/default
ADD nginx.conf /etc/nginx/nginx.conf
ADD php-fpm.conf /etc/php/7.3/fpm/php-fpm.conf
ADD www.conf /etc/php/7.3/fpm/pool.d/www.conf
ADD .bashrc /root/.bashrc
ADD composer.phar /usr/local/bin/composer
ADD xdebug.ini /etc/php/7.3/fpm/conf.d/xdebug.ini

# Add startup scripts for services
ADD nginx.sh /etc/service/nginx/run
RUN chmod +x /etc/service/nginx/run

ADD phpfpm.sh /etc/service/phpfpm/run
RUN chmod +x /etc/service/phpfpm/run

ADD ntp.sh /etc/service/ntp/run
ADD ntp.conf /etc/ntp.conf
RUN chmod +x /etc/service/ntp/run

# Set WWW public folder
RUN mkdir -p /var/www

RUN chown -R www-data:www-data /var/www
RUN chmod -R 755 /var/www

# Add New Relic APM install script
RUN mkdir -p /etc/my_init.d
ADD newrelic.sh /etc/my_init.d/newrelic.sh
RUN chmod +x /etc/my_init.d/newrelic.sh

# Setup environment variables for initializing New Relic APM
ENV NR_INSTALL_SILENT 1
ENV NR_INSTALL_KEY **ChangeMe**
ENV NR_APP_NAME "Docker PHP Application"

# Set terminal environment
ENV TERM=xterm

# Port and settings
EXPOSE 80

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
