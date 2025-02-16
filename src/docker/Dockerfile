ARG phpVersion=8.2
FROM php:${phpVersion}-fpm
ARG WORKDIR=/var/www/html
ENV DOCUMENT_ROOT=${WORKDIR}
ARG GROUP_ID=1000
ARG USER_ID=1000
ENV USER_NAME=www-data
ARG GROUP_NAME=www-data
# Install system dependencies
RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -y \
    bzip2 \
    cron \
    curl \
    git \
    imagemagick \
    libmagickwand-dev \
    libbz2-dev \
    libc-client-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libmemcached-dev \
    libkrb5-dev \
    libonig-dev \
    libpq-dev \
    libzip-dev \
    libxml2-dev \
    net-tools \
    nginx \
    libldap-dev \
    openssh-server \
    pigz \
    sqlite3  \
    sudo \
    supervisor \
    vim\
    zip \
    unzip && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-configure ldap && \
    docker-php-ext-install -j$(nproc) bcmath bz2 exif gd gettext imap intl ldap mbstring mysqli opcache pdo_mysql pdo_pgsql pcntl zip && \
    pecl install apcu imagick memcached redis  && \
    docker-php-ext-enable apcu imagick memcached redis && \
    pecl clear-cache

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set working directory
WORKDIR $WORKDIR

COPY src/php/apcu.ini    $PHP_INI_DIR/conf.d/
COPY src/php/php.ini     $PHP_INI_DIR/conf.d/
COPY src/php/opcache.ini $PHP_INI_DIR/conf.d/
COPY src/php/www.conf    /usr/local/etc/php-fpm.d/
COPY src/supervisord.conf /etc/supervisor/supervisord.conf

COPY src/docker/entrypoint.sh /usr/local/bin/
COPY src/nginx/nginx.conf /etc/nginx/nginx.conf
COPY src/nginx/default.conf /etc/nginx/conf.d/

RUN rm -Rf /var/www/* && \
    mkdir -p /var/www/html && \
    chmod +x /usr/local/bin/entrypoint.sh && \
    ln -s /usr/local/bin/entrypoint.sh / && \
    mkdir -p /var/log/supervisor /var/log/nginx && \
    chown -R ${USER_NAME}:${GROUP_NAME} /var/www && \
    chown -R ${USER_NAME}:${GROUP_NAME} /var/log/ && \
    chown -R ${USER_NAME}:${GROUP_NAME} /etc/supervisor/conf.d/ && \
    chown -R ${USER_NAME}:${GROUP_NAME} $PHP_INI_DIR/conf.d/ && \
    touch /var/run/nginx.pid && \
    chown -R ${USER_NAME}:${GROUP_NAME} /var/lib/nginx/ && \
    chown -R ${USER_NAME}:${GROUP_NAME} /etc/nginx/nginx.conf && \
    chown -R ${USER_NAME}:${GROUP_NAME} /var/run/nginx.pid && \
    chown -R ${USER_NAME}:${GROUP_NAME} /var/log/supervisor && \
    chown -R ${USER_NAME}:${GROUP_NAME} /etc/nginx/conf.d/ && \
    chown -R ${USER_NAME}:${GROUP_NAME} /tmp

#USER ${USER_NAME}
EXPOSE 80
ENTRYPOINT ["entrypoint.sh"]
