FROM nginx:latest

MAINTAINER badtudou <badtudou@gmail.com>

COPY ./www/index.html /usr/share/nginx/html/index.html

#COPY ./ssl /etc/nginx/ssl

#COPY ./conf/conf.d /etc/nginx/conf.d

VOLUME ["/www"]

VOLUME ["/ssl"]

EXPOSE 80

EXPOSE 443

EXPOSE 8080