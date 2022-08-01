FROM php:7.4-fpm

# Maintainer
LABEL maintainer="Dionnys Bonalde"

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    vim \
    wget \
    htop \
    iotop \
    lsof \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libxslt-dev \
    zlib1g-dev \
    libzip-dev \
    libicu-dev \
    libmcrypt-dev \
    libmagickwand-dev \
    libbz2-dev \
    libgmp-dev \
    libc-client-dev\
    libkrb5-dev \
    zip \
    unzip \
    && pecl install \
    mcrypt-1.0.4 \
    rar \
    imagick


# Install PHP extensions
RUN docker-php-ext-install -j$(nproc) xmlrpc xsl calendar exif sockets mysqli gettext \
    pdo_mysql mbstring exif pcntl bcmath gd soap zip intl bz2 gmp

RUN docker-php-ext-configure intl \
    && docker-php-ext-configure  imap --with-imap --with-imap-ssl --with-kerberos \
    && docker-php-ext-install imap shmop sysvmsg sysvsem sysvshm\
    && docker-php-ext-enable mcrypt imagick rar

# Install opcache, xdebug, redis, mongodb
RUN docker-php-source extract \
    && pecl install opcache xdebug redis mongodb apcu \
    && echo "xdebug.remote_enable=on\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=on\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=9000\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_connect_back=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && docker-php-ext-enable opcache xdebug redis mongodb apcu \
    && docker-php-source delete \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#setup composer
RUN EXPECTED_CHECKSUM="$(wget -q -O - https://composer.github.io/installer.sig)" && \
	php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
	ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")" && \
	if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]; then >&2 echo 'ERROR: Invalid installer checksum' && exit 1; fi

#copy environment variables
COPY .env /apps/

WORKDIR /apps/public_html


# Clean up
RUN apt-get remove -y git && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
