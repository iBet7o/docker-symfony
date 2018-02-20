# See https://github.com/docker-library/php/blob/master/7.1/fpm/Dockerfile
FROM php:7.1-fpm

MAINTAINER Roberto Ramírez <robertoiran@gmail.com>

RUN apt-get update && apt-get install -y \
    git \
    unzip \
    vim \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng12-dev \
    wget \
    zlib1g-dev \
    libicu-dev \
    g++ \
    libxml2-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install soap

# Install wkhtmltopdf
RUN apt-get install -y libfontconfig1 libxrender1
RUN wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
RUN tar -vxf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
RUN rsync -vlr wkhtmltox/* /usr/local/
RUN ln -s /usr/local/bin/wkhtmltopdf /usr/bin/wkhtmltopdf
RUN ln -s /usr/local/bin/wkhtmltoimage /usr/bin/wkhtmltoimage

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
RUN ln -s /usr/share/zoneinfo/Mexico/City /etc/localtime
RUN "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql

# Install Nodejs
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs

# Install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Install deployer
RUN curl -LO https://deployer.org/deployer.phar
RUN mv deployer.phar /usr/local/bin/dep
RUN chmod +x /usr/local/bin/dep

# Configurations php
RUN echo "upload_max_filesize = 15M" >> /usr/local/etc/php/conf.d/docker-php.ini
RUN echo "post_max_size = 15M" >> /usr/local/etc/php/conf.d/docker-php.ini
RUN echo "memory_limit = -1" >> /usr/local/etc/php/conf.d/docker-php.ini
RUN echo "date.timezone = America/Mexico_City" >> /usr/local/etc/php/conf.d/docker-php.ini

WORKDIR /var/www/
