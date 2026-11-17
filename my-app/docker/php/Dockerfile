FROM php:8.3-fpm-alpine

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

RUN apk add --no-cache \
        freetype \
        libjpeg-turbo \
        libpng \
        libpq \
        libzip \
    && apk add --no-cache --virtual .build-deps \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libzip-dev \
        postgresql-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j"$(nproc)" \
        bcmath \
        gd \
        opcache \
        pdo_pgsql \
        pgsql \
        zip \
    && apk del .build-deps

RUN { \
        echo 'memory_limit=512M'; \
        echo 'upload_max_filesize=512M'; \
        echo 'post_max_size=512M'; \
        echo 'max_file_uploads=20'; \
    } > /usr/local/etc/php/conf.d/uploads.ini

WORKDIR /app
