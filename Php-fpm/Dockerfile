FROM php:7.1-fpm

MAINTAINER badtudou <badtudou@gmail.com>

# Install all required packages.
#RUN sed -i "s/deb.debian.org/mirrors.ustc.edu.cn/g" /etc/apt/sources.list 
#RUN sed -i 's/security.debian.org/mirrors.ustc.edu.cn\/debian-security/g' /etc/apt/sources.list
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        zip \
        supervisor \
        cron \
        #libpng12-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt zip pdo_mysql mbstring bcmath zip opcache\
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && rm -rf /var/lib/apt/lists/* \
    && sed -i 's/^\(\[supervisord\]\)$/\1\nnodaemon=true/' /etc/supervisor/supervisord.conf

COPY ./conf/php.ini /etc/php.ini

COPY ./conf/php.d /etc/php.d

COPY ./conf/php-fpm.d /etc/php-fpm.d

COPY ./conf/php-fpm.conf /etc/php-fpm.conf

#install Composer
RUN php -r "copy('http://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/bin --filename=composer
RUN php -r "unlink('composer-setup.php');" \
&& composer config -g repo.packagist composer https://packagist.phpcomposer.com \
&& composer global require "laravel/installer" \
&& echo export PATH="~/.composer/vendor/bin:$PATH" >> ~/.bashrc

# Run the command on container startup
COPY ./conf/cron.d /etc/cron.d
RUN chmod 0644 /etc/cron.d/*

WORKDIR "/usr/share/nginx/html"

# Define mountable directories.
VOLUME ["/etc/supervisor/conf.d"]

VOLUME ["/usr/share/nginx/html"]

VOLUME ["/etc/cron.d"]

EXPOSE 9000

CMD ["supervisord"]

