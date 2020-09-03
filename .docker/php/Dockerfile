FROM php:7.3-fpm-alpine

WORKDIR /var/www

RUN apk update && apk add build-base libzip-dev libpng-dev libjpeg-turbo-dev zip unzip jpegoptim optipng pngquant gifsicle vim unzip git curl composer

RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip

RUN docker-php-ext-install pdo pdo_mysql pcntl bcmath exif mysqli

RUN apk add autoconf && pecl install -o -f redis \
&& rm -rf /tmp/pear \
&& docker-php-ext-enable redis && apk del autoconf

#RUN apk add autoconf && pecl install -o -f redis \
#&&  rm -rf /tmp/pear \
#&&  docker-php-ext-enable redis && apk del autoconf
#
COPY .docker/php/local.ini /usr/local/etc/php/conf.d/local.ini
RUN rm -rf .git

RUN addgroup -g 1000 -S www && \
    adduser -u 1000 -S www -G www

USER 1000

COPY --chown=www:www . /var/www

COPY docker-entrypoint.sh /docker-entrypoint.sh

ENTRYPOINT ["sh", "/docker-entrypoint.sh"]

EXPOSE 9000

CMD ./docker-entrypoint.sh
