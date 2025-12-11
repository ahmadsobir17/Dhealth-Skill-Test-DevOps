FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    php \
    php-cli \
    php-mbstring \
    php-xml \
    php-pgsql \
    php-intl \
    php-curl \
    unzip \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html

RUN rm -rf /var/www/html/*

RUN composer create-project --prefer-dist yiisoft/yii2-app-basic .

EXPOSE 3000

CMD ["php", "yii", "serve", "0.0.0.0", "--port=3000", "--docroot=web"]