FROM php:8.1.3-fpm-bullseye

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends \
        git \
        libpq-dev \
        libxml2-dev \
        libzip-dev \
        locales \
        tzdata 
RUN apt-get autoremove -y && \
    apt-get clean && \
	rm -rf /var/lib/apt/lists/*
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    sed -i -E 's/# (ja_JP.UTF-8)/\1/' /etc/locale.gen && \
    locale-gen && \
    update-locale LANG=ja_JP.UTF-8
RUN pecl install \
    xdebug-3.1.3
RUN docker-php-ext-install \
    pdo_pgsql \
    xml \
    zip
RUN docker-php-ext-enable \
    xdebug
COPY --from=composer /usr/bin/composer /usr/bin/composer
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
ARG LARAVEL_UID=1000
ARG LARAVEL_USER=laravel
ARG LARAVEL_HOME=/home/laravel
ARG LARAVEL_DIR=/laravel
RUN useradd -g www-data -u ${LARAVEL_UID} -d ${LARAVEL_HOME} -m ${LARAVEL_USER} && \
    mkdir -p ${LARAVEL_DIR} /var/run/php && \
    chown ${LARAVEL_USER}:www-data ${LARAVEL_DIR} /var/run/php
COPY zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf
