# Menggunakan base image Ubuntu Latest 
FROM ubuntu:latest

# Mencegah interaksi interaktif saat install (supaya tidak nanya timezone dll)
ENV DEBIAN_FRONTEND=noninteractive

# Update repository ubuntu dan install PHP serta dependensi untuk Yii 2.0
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

# Install Composer 
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Set folder kerja di dalam container
WORKDIR /var/www/html

# Kita hapus semua file (biasanya index.html bawaan apache) agar folder bersih
RUN rm -rf /var/www/html/*

# Install Yii 2.0 Basic Template
# (Ini akan mendownload Yii ke dalam container)
RUN composer create-project --prefer-dist yiisoft/yii2-app-basic .

# Ekspos port 3000 (Hanya deklarasi, pemetaan asli ada di docker-compose)
EXPOSE 3000

# Perintah agar container tetap jalan dan menyalakan server bawaan Yii di port 3000
CMD ["php", "yii", "serve", "0.0.0.0", "--port=3000", "--docroot=web"]