# Source

link :

```bash
https://inertiajs.com/docs/v3/getting-started/index
```

# instalasi

instalasi dibagi menjadi 2:

1. Server setup => ini digunakan instalasi di BE nya , disini konteksnya berarti install di sisi laravelnya
2. Client Setuo => ini diinstall di framework FE nya ( bisa vue atau react )
   keduanya harus diinstall agar antara BE dan FE bisa terhubung

# Server Setup

install dulu :

```bash
composer require inertiajs/inertia-laravel

```

setelah selesai update/edit `resources/views/app.blade.php` menjadi :

```php
 <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1">
        @vite('resources/js/app.js')
        <x-inertia::head />
    </head>
    <body>
        <x-inertia::app />
    </body>
</html>
```

ini berarti default bawaan app laravel sudah berhasil di konversi ke inertia.

setelah itu install middleware inertia nya :

```bash
php artisan inertia:middleware
```

kemudian set middleware `web` pada `bootstrap/app.php` dengan kode ini :

```php
use App\Http\Middleware\HandleInertiaRequests;

->withMiddleware(function (Middleware $middleware) {
    $middleware->web(append: [
        HandleInertiaRequests::class,
    ]);
})
```

# Client Setup

disini saya menggunakan `vue.js` , install vue nya :

```bash
npm install vue @vitejs/plugin-vue
```

lalu tambahkan vue di `vite.config.js` :

```js
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";
import vue from "@vitejs/plugin-vue";

export default defineConfig({
  plugins: [
    laravel({
      input: ["resources/js/app.js"],
      refresh: true,
    }),
    vue(),
  ],
});
```

kamu cukup menambahkan :

```js
import vue from "@vitejs/plugin-vue";
```

dan

```js
vue();
```

selanjutnya adalah konfigurasi dependencies :

# install dependencies

install :

```bash
npm install @inertiajs/vue3 @inertiajs/vite
```

lalu edit kembali `vite.config.js` :

```js
import inertia from "@inertiajs/vite";
import laravel from "laravel-vite-plugin";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [
    laravel({
      input: ["resources/js/app.js"],
      refresh: true,
    }),
    inertia(),
  ],
});
```

yang kamu tambahkan :

```js
import inertia from "@inertiajs/vite";
```

dan

```js
 inertia(),
```

# integrasi

edit dulu `app.js` atau file js utama yang digunakan :

```js
import { createApp, h } from "vue";
import { createInertiaApp } from "@inertiajs/vue3";

createInertiaApp({
  resolve: (name) => {
    const pages = import.meta.glob("./Pages/**/*.vue");
    return pages[`./Pages/${name}.vue`]();
  },
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el);
  },
});
```

disini saya mengasumsikan semua view berada pada folder `Pages`.
buat file nya dulu dengan struktur misal `Pages/Dashboard.vue` dan isi dengan starter template vue.

```js
<script setup></script>
<template>
    <div>Dashboard</div>
</template>
<style scoped></style>

```

kemudian disisi laravel nya kita coba membuat controller baru untuk melihat hasil :

```bash
php artisan make:controller DashboardController
```

edit controller tambahkan endpoint,misal :

```php
 function index()
    {
        return Inertia::render('Home', []);
    }
```

buat route nya :

```php

Route::get('/', [DashboardController::class, 'index']);
```

# BOOOMS

seharusnya sudah terhubung
