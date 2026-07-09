FROM php:8.1-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    && docker-php-ext-install pdo_mysql

WORKDIR /var/www

COPY . .

# Install PHP dependencies
RUN composer install

CMD ["php-fpm"]