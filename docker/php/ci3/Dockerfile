FROM php:8.2-fpm

# Arguments defined in docker-compose.yml
ARG APP_PATH
ARG APP_PORT

# Install system dependencies
RUN apt-get update && apt-get install -y \
        net-tools \
        procps \
        git \
        curl \
        libpng-dev \
        libonig-dev \
        libxml2-dev \
        zip \
        unzip \
        build-essential \
        autoconf \
        libz-dev \
        libzip-dev \
        libssl-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo_mysql mbstring exif pcntl bcmath gd zip

# Set working directory
WORKDIR /var/www/$APP_PATH

# Copy existing project files
COPY ../../$APP_PATH .

# Set correct permissions
RUN chown -R www-data:www-data /var/www/$APP_PATH \
    && chmod -R 775 /var/www/$APP_PATH

# Copy custom php.ini
COPY ../../docker/php/ci3/php.ini /usr/local/etc/php/conf.d/memory-limit.ini

# Update php-fpm to listen on APP_PORT instead of default 9000
RUN sed -i "s/9000/${APP_PORT}/" /usr/local/etc/php-fpm.d/zz-docker.conf

# Expose the specified port
EXPOSE $APP_PORT

CMD ["php-fpm"]
