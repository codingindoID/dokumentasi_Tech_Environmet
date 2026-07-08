<h1>Auth With Sanctum</h1>

# Arsitektur

```text
Nuxt 4
    │
    │ POST /api/login
    ▼
Laravel API
    │
    │ validasi email & password
    ▼
Sanctum
    │
    │ membuat token
    ▼
Response
{
   token: "...",
   user: {...}
}

Nuxt menyimpan token
        │
        ▼
Authorization: Bearer xxxxxxxxx
```

# STEP 1 - Install Sanctum

```bash
composer require laravel/sanctum
```

Publish konfigurasi

```bash
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
```

migrasi databse

```bash
php artisan migrate
```

# STEP 2 - User Model

di Model User tambahkan :

```php
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens;

    ...
}
```

jika kamu menggunakan resource API gunakan ini :

```bash
php artisan install:api
```

ini menambahkan file di `routes/api`
