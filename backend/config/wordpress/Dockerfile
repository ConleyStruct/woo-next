FROM composer/composer:php7 as build

# Install composer
WORKDIR /var/www/html/wp-content

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer

COPY composer.json /var/www/html/wp-content
# COPY composer.lock ./
RUN composer install --no-scripts --no-autoloader
# COPY ./ /var/www/
RUN composer dump-autoload --optimize
RUN composer update 

FROM wordpress:5.1.1-php7.3-apache

COPY --from=build /var/www/html/wp-content /var/www/html/wp-content

# Expose a port to run on
EXPOSE 80