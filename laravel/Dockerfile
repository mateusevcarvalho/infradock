ARG PHP_VERSION_LARAVEL=${PHP_VERSION_LARAVEL}
ARG INSTALL_MONGO_LARAVEL=${INSTALL_MONGO_LARAVEL}
ARG INSTALL_XDEBUG_LARAVEL=${INSTALL_XDEBUG_LARAVEL}

FROM php:${PHP_VERSION_LARAVEL}-apache

RUN apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends \
    git \
    unzip \
    procps \
    nano \
    vim \
    libcurl4 \
    libcurl4-openssl-dev \
    pkg-config \
    libicu-dev \
    libssl-dev \
    zlib1g-dev \
    libxml2 \
    libxml2-dev \
    cron \
    && rm -rf /var/lib/apt/lists

# Enable PHP extensions
RUN docker-php-ext-install pdo_mysql mysqli mbstring intl

# Install GD
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

#Install mcrypt
RUN apt-get install -y libmcrypt-dev libreadline-dev
RUN pecl install mcrypt-1.0.2
RUN docker-php-ext-enable mcrypt

#Install zip
RUN apt-get install -y \
    libzip-dev \
    zip \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

ARG INSTALL_XDEBUG_LARAVEL=false
ENV INSTALL_XDEBUG_LARAVEL ${INSTALL_XDEBUG_LARAVEL}

RUN if [ ${INSTALL_XDEBUG_LARAVEL} = true ]; then \
    #Install xdebug
    pecl install xdebug && docker-php-ext-enable xdebug \
;fi


ARG INSTALL_MONGO_LARAVEL=false
ENV INSTALL_MONGO_LARAVEL ${INSTALL_MONGO_LARAVEL}

RUN if [ ${INSTALL_MONGO_LARAVEL} = true ]; then \
    # install mongodb ext
    pecl install mongodb && docker-php-ext-enable mongodb \
;fi

# Create tmp directory and make it writable by the web server
RUN mkdir -p \
    tmp/cache/models \
    tmp/cache/persistent \
    && chown -R :www-data \
    tmp \
    && chmod -R 770 \
    tmp

ADD ./apache/000-default.conf /etc/apache2/sites-available/

# Enable Apache modules and restart
RUN a2enmod rewrite \
    && service apache2 restart

EXPOSE 80
