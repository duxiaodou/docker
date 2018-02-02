FROM nginx:latest

MAINTAINER badtudou <badtudou@gmail.com>

COPY ./www/index.html /usr/share/nginx/html/index.html

COPY ./ssl/server.crt /etc/pki/nginx/server.crt

COPY ./ssl/server.key /etc/pki/nginx/private/server.key

COPY ./conf/conf.d /etc/nginx/conf.d

VOLUME ["/www"] 

EXPOSE 80

EXPOSE 443