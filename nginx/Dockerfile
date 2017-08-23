FROM debian:jessie

MAINTAINER Roberto Ramírez <robertoiran@gmail.com>

RUN apt-get update && apt-get install -y \
    nginx \
    vim

ADD nginx.conf /etc/nginx/

RUN rm /etc/nginx/sites-enabled/default /etc/nginx/sites-available/default

ADD vhosts/*.conf /etc/nginx/sites-available/
RUN for vhost in /etc/nginx/sites-available/*; do ln -s $vhost /etc/nginx/sites-enabled/$(basename $vhost); done

RUN echo "upstream php-upstream { server php:9000; }" > /etc/nginx/conf.d/upstream.conf

RUN usermod -u 1000 www-data

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
