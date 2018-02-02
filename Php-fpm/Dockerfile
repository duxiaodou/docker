FROM php:7.2-fpm

MAINTAINER badtudou <badtudou@gmail.com>

# Install all required packages.
RUN sed -i "s/deb.debian.org/mirrors.aliyun.com/g" /etc/apt/sources.list 
RUN sed -i "s/security.debian.org/mirrors.aliyun.com\/debian-security/g" /etc/apt/sources.list
RUN apt-get update --fix-missing && \
  apt-get install \
		git \
		libldap2-dev \
		libcurl4-gnutls-dev \
		curl \
		libicu-dev \
		libmcrypt-dev \
		libvpx-dev \
		libjpeg-dev \
		libpng-dev \
		libxpm-dev \
		zlib1g-dev \
		libfreetype6-dev \
		libxml2-dev \
		libexpat1-dev \
		libbz2-dev \
		libgmp3-dev \
		libldap2-dev \
		unixodbc-dev \
		libpq-dev \
		libsqlite3-dev \
		libaspell-dev \
		libsnmp-dev \
		libpcre3-dev \
		libtidy-dev \
		build-essential \
		libkrb5-dev \
		libedit-dev \
		libedit2 \
		gcc \
		make \
		python2.7-dev \
		python-pip \
		re2c \
		wget \
		sqlite3 \
		libmemcached-dev \
		libc-client-dev -yqq \
	&& rm -rf /var/lib/apt/lists/* 
# Compile PHP, include these extensions.
RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ 
RUN docker-php-ext-configure imap --with-imap-ssl --with-kerberos --with-imap
RUN docker-php-ext-install mbstring \
   pdo_mysql \
   json \
   intl \
   gd \
   xml \
   zip \
   bz2 \
   opcache \
   pgsql \
   pdo_sqlite\
   intl \
   bcmath \
   soap \
   ldap \
   imap \
   readline

# Compile and install APCu
RUN pecl install apcu \
    && docker-php-ext-enable apcu

#install Imagemagick & PHP Imagick ext
RUN apt-get update && apt-get install -y \
  libmagickwand-dev --no-install-recommends
RUN pecl install imagick && docker-php-ext-enable imagick

COPY ./conf/php.ini /etc/php.ini

COPY ./conf/php.d /etc/php.d

COPY ./conf/php-fpm.d /etc/php-fpm.d

COPY ./conf/php-fpm.conf /etc/php-fpm.conf

#install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php \
&& php -r "unlink('composer-setup.php');" \
&& mv composer.phar /usr/local/bin/composer

EXPOSE 9000