FROM php:8.2-apache

# Enable Apache Rewrite Module
RUN a2enmod rewrite

# Install PHP Extensions
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd pdo pdo_mysql

# Set the working directory
WORKDIR /var/www/html

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy the application code
COPY . .

# Copy custom Apache configuration file
COPY apache.conf /etc/apache2/sites-available/000-default.conf


# Install dependencies
RUN rm -rf /var/www/html/vendor
RUN rm -r /var/www/html/composer.lock
RUN composer install --no-dev --no-cache

RUN cp .env.example .env

# Modify the .env file
RUN sed -i 's/DB_CONNECTION=mysql/DB_CONNECTION=sqlite/g' .env \
    && sed -i 's/DB_DATABASE=laravel/DB_DATABASE=\/var\/www\/html\/database\/database.sqlite/g' .env \
    && sed -i 's/APP_DEBUG=false/APP_DEBUG=true/g' .env

# Generate application key
RUN php artisan key:generate

# Clear configuration cache
RUN php artisan config:clear

# Set proper permissions
RUN chown -R www-data:www-data /var/www/html


RUN chmod -R 775 /var/www/html/bootstrap/
RUN chmod -R 775 /var/www/html/storage/


RUN chown -R www-data:www-data /var/www/html/bootstrap/
RUN chown -R www-data:www-data /var/www/html/storage/


RUN chmod -R 775 /var/www/html/database
RUN chown -R www-data:www-data /var/www/html/database

RUN php artisan migrate

#RUN php artisan db:seed

RUN php artisan storage:link

RUN php artisan o:


# Define the default command to run Apache
CMD ["apache2-foreground"]

# Expose port 80
EXPOSE 80
