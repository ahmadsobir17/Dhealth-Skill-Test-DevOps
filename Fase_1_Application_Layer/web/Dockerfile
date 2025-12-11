FROM ubuntu:latest

RUN apt-get update && apt-get install -y \
    php8.3 \
    php8.3-cli \
    php8.3-mbstring \
    php8.3-xml \
    php8.3-pgsql \
    php8.3-intl \
    php8.3-curl \
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