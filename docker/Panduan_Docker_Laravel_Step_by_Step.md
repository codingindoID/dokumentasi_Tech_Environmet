# Panduan Docker Laravel (Step by Step)

## Tujuan

Menjalankan project Laravel menggunakan Docker dengan: - PHP-FPM -
Nginx - MySQL

------------------------------------------------------------------------

# Struktur Project

``` text
praktekLaravelDocker/
├── app/
├── bootstrap/
├── config/
├── docker/
│   └── nginx/
│       └── default.conf
├── public/
├── routes/
├── storage/
├── Dockerfile
├── compose.yaml
└── .env
```

------------------------------------------------------------------------

# 1. Dockerfile

Dockerfile digunakan untuk **membuat image PHP**.

Fungsinya: - Menentukan versi PHP - Install extension PHP - Install
Composer - Menentukan working directory

Contoh:

``` dockerfile
FROM php:8.3-fpm

RUN apt-get update && apt-get install -y \
    git unzip zip curl libzip-dev libpng-dev libonig-dev libxml2-dev

RUN docker-php-ext-install pdo_mysql mysqli zip

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www
```

------------------------------------------------------------------------

# 2. compose.yaml

Compose digunakan untuk menjalankan beberapa container sekaligus.

Pada project ini ada 3 service:

-   app (PHP)
-   nginx (Web Server)
-   mysql (Database)

Contoh mapping:

``` yaml
ports:
  - "8080:80"
```

Artinya:

Host (komputer) `8080` → Container Nginx `80`

------------------------------------------------------------------------

# 3. Nginx (default.conf)

File:

``` text
docker/nginx/default.conf
```

Hal penting:

``` nginx
listen 80;
root /var/www/public;

fastcgi_pass app:9000;
```

Yang perlu diingat:

-   `listen 80` → port di dalam container.
-   `root` harus menunjuk ke folder `public`.
-   `app:9000` adalah nama service pada compose, bukan localhost.

------------------------------------------------------------------------

# 4. Docker Network

Docker otomatis membuat network.

Semua service saling mengenal berdasarkan nama service.

Misalnya:

-   mysql
-   app
-   nginx

Karena itu Laravel menggunakan:

``` env
DB_HOST=mysql
```

Bukan:

``` env
DB_HOST=127.0.0.1
```

------------------------------------------------------------------------

# 5. Konfigurasi .env

Contoh:

``` env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

Kenapa bukan localhost?

Karena database berada pada container lain.

------------------------------------------------------------------------

# 6. Menjalankan Project

Build pertama:

``` bash
docker compose up -d --build
```

Melihat container:

``` bash
docker ps
```

Masuk ke container PHP:

``` bash
docker exec -it laravel_app bash
```

------------------------------------------------------------------------

# 7. Membersihkan Cache Laravel

Jika mengubah `.env`:

``` bash
php artisan optimize:clear
php artisan config:cache
```

------------------------------------------------------------------------

# 8. Yang Harus Diingat

## Dockerfile

-   Membuat image.
-   Dijalankan saat build.

## compose.yaml

-   Menjalankan container.
-   Menghubungkan service.
-   Mapping port.
-   Volume.
-   Environment.

## Volume

``` yaml
volumes:
  - ./:/var/www
```

Kode di Windows otomatis muncul di container.

## Port

``` yaml
8080:80
3307:3306
```

Format:

    HOST:CONTAINER

## Service Name

Nama service digunakan sebagai hostname.

Contoh:

``` yaml
services:
  mysql:
```

Maka:

``` env
DB_HOST=mysql
```

## Container

-   laravel_app
-   laravel_nginx
-   laravel_mysql

## Perintah Penting

``` bash
docker compose up -d --build
docker compose down
docker compose restart
docker ps
docker logs laravel_app
docker logs laravel_nginx
docker exec -it laravel_app bash
```

------------------------------------------------------------------------

# Alur Kerja Request

    Browser
       │
       ▼
    Host:8080
       │
    Docker Port Mapping
       │
    Nginx
       │
    PHP-FPM (app)
       │
    Laravel
       │
    MySQL

------------------------------------------------------------------------

# Error yang Sering Terjadi

## Connection refused

Penyebab: - DB_HOST masih 127.0.0.1

Solusi:

``` env
DB_HOST=mysql
```

Lalu:

``` bash
php artisan optimize:clear
```

## 502 Bad Gateway

Biasanya: - app container mati - fastcgi_pass salah

## 404

Biasanya: - root bukan `/public`

------------------------------------------------------------------------

# Kesimpulan

Ingat urutan berikut:

1.  Dockerfile → membuat image.
2.  compose.yaml → menjalankan container.
3.  default.conf → konfigurasi Nginx.
4.  .env → konfigurasi Laravel.
5.  `docker compose up -d --build`
6.  `php artisan optimize:clear` bila `.env` berubah.
