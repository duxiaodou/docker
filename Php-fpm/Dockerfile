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
        wget \
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
RUN wget https://raw.githubusercontent.com/composer/getcomposer.org/76a7060ccb93902cd7576b67264ad91c8a2700e2/web/installer -O - -q | php -- --quiet \
RUN php -r "unlink('composer-setup.php');" \
&& mv composer.phar /usr/local/bin/composer \
&& composer config -g repo.packagist composer https://packagist.phpcomposer.com \
&& composer global require "laravel/installer" \
&& echo export PATH="~/.composer/vendor/bin:$PATH" >> ~/.bashrc

# Run the command on container startup
COPY ./conf/cron.d /etc/cron.d

WORKDIR "/usr/share/nginx/html"

RUN chown -R www-data:www-data /usr/share/nginx/html \
&& chmod -R g+rwX /usr/share/nginx/html

# Define mountable directories.
VOLUME ["/etc/supervisor/conf.d"]

VOLUME ["/usr/share/nginx/html"]

VOLUME ["/etc/cron.d"]

EXPOSE 9000

CMD ["supervisord"]

