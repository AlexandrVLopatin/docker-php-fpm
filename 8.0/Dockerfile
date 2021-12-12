FROM php:8.0-fpm

# Install dependencies
RUN set -ex; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
    busybox-static \
    iputils-ping \
    unzip \
    git \
    ssh-client \
    sudo \
    nginx \
    supervisor \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxml2-dev; \
    apt-get autoclean -y; \
    apt-get clean -y;

# Install PHP extensions
RUN set -ex; \
    docker-php-ext-configure gd --with-jpeg --with-freetype; \
    docker-php-ext-install -j$(nproc) \
    gd \
    bcmath \
    zip \
    pdo_mysql \
    mysqli \
    opcache \
    simplexml; \
    docker-php-source delete; \
    php -m;

# Copy php.ini
RUN cp "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# Install Composer
COPY --from=composer:2.1 /usr/bin/composer /usr/bin/composer

RUN set -ex; \
    chmod +x /usr/bin/composer; \
    mkdir /var/www/.composer; \
    chown www-data:www-data -R /var/www/.composer; \
    composer --version;

EXPOSE 9000
CMD ["php-fpm"]
