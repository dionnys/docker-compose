FROM php:8.1-fpm

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
    unzip



# Install PHP extensions
RUN docker-php-ext-install -j$(nproc)  xsl calendar exif sockets mysqli gettext \
    pdo_mysql exif pcntl bcmath gd soap zip intl bz2 gmp




# Install opcache, xdebug, redis, mongodb
RUN docker-php-source extract \
    && pecl install opcache xdebug redis mongodb apcu \
    && docker-php-ext-enable opcache xdebug redis mongodb apcu \
    && docker-php-source delete \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#setup composer
RUN EXPECTED_CHECKSUM="$(wget -q -O - https://composer.github.io/installer.sig)" && \
	php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
	ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")" && \
	if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]; then >&2 echo 'ERROR: Invalid installer checksum' && exit 1; fi

#copy environment variables
#COPY .env /apps/

WORKDIR /apps/public_html


# Clean up
RUN apt-get remove -y git && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
