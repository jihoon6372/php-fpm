FROM php:8.4-fpm

WORKDIR /var/www/html

# PHP 확장 설치
RUN apt-get update && apt-get install -y \
    # 빌드에 필요한 개발 패키지들
    libpng-dev libjpeg-dev libwebp-dev libfreetype6-dev \
    zlib1g-dev libzip-dev libonig-dev libxml2-dev \
    libcurl4-openssl-dev libjpeg62-turbo-dev libonig-dev \
    && docker-php-ext-install pdo pdo_mysql mbstring bcmath mysqli opcache zip exif \
    && pecl install redis && docker-php-ext-enable redis \
    && docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install gd \
    # 빌드 의존성 제거 (이미지 크기 감소, 런타임 라이브러리는 자동 유지됨)
    && apt-get purge -y --auto-remove \
        libcurl4-openssl-dev \
        zlib1g-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libwebp-dev \
        libonig-dev \
        libzip-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

EXPOSE 9000