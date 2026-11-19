
# Use the official PHP 8.3 image with PHP-FPM.
# PHP-FPM manages PHP processes and executes PHP requests received from Nginx.
FROM php:8.3-fpm

# Set the working directory inside the container.
# All subsequent commands will run from this directory.
WORKDIR /var/www/html

# Install system dependencies and libraries required by PHP extensions.
RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    zip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    && docker-php-ext-install \
    pdo_mysql \
    mbstring \
    exif \
    pcntl \
    bcmath \
    gd \
    zip \
    opcache \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


# Copy Composer from the official Composer image.
# This avoids installing Composer manually inside the container.
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copy the Laravel application source code into the container.
COPY . .

# Give the PHP-FPM user ownership of Laravel's writable directories.
# Laravel needs write permissions for logs, cache, and compiled files.
RUN chown -R www-data:www-data storage bootstrap/cache

# Expose port 9000 for PHP-FPM communication.
# Nginx will communicate with PHP-FPM through the Docker network.
EXPOSE 9000

# Start PHP-FPM when the container launches.
CMD ["php-fpm"]
