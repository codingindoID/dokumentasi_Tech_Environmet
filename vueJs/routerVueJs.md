# Instalasi

source :

```php
https://router.vuejs.org/
```

install via npm :

```bash
npm install vue-router@4
```

# konfigurasi

daftarkan router pada main.js :

```js
import { createApp } from "vue";
import App from "./App.vue";
import router from "@/router";

createApp(App).use(router).mount("#app");
```

# Folder Router Index

Buat folder baru untuk tampungan routerindex , contoh :

```bash
src/router/index.js
```

kemudian isi kode nya, pada contoh kali ini saya menggunakan 2 template yaitu :

```bash
BlankLayout = untuk layout tanpa navbar
NavbarLayout = untuk template dengan navbar
```

misal strukturnya di sini :

```bash
src/layout/BlankLayout.vue
src/layout/NavbarLayout.vue
```

contoh BlankLayout :

```js
<script setup></script>
<template>
  <RouterView />
</template>
<style scoped></style>

```

contoh nabvarLayout :

```js
<script setup>
import Navbar from '../components/NavBar.vue'
</script>
<template>
  <Navbar />
  <RouterView />
</template>
<style scoped></style>

```

code minimalnya, misal saya punya 2 viw home, dan login :
lakukan import didalam komponen agar tidak selalu di load saat pertama kali

```js
import { createWebHistory, createRouter } from "vue-router";
import BlankLayout from "@/layout/BlankLayout.vue";
import NavbarView from "@/layout/NavbarView.vue";

const routes = [
  {
    path: "/",
    component: NavbarView,
    name: "NavbarLayout",
    children: [
      {
        path: "",
        name: "defaultpage",
        component: () => import("../views/Home/Index.vue"),
      },
    ],
  },
  {
    path: "/login",
    name: "BlankLayout",
    component: BlankLayout,
    children: [
      {
        path: "/login",
        name: "login",
        component: () => import("../views/LoginView.vue"),
      },
    ],
  },
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

# setUp Router Parent untuk Router View

pada App.vue ( parent Aplikasi ) setting Router View

```js
<script setup></script>

<template>
  <RouterView />
</template>

<style scoped></style>

```

# Khusus jika Kamu Menggunakan Verssi TS

tambahkan settingan config ini di `tsconfig.json/env.d.ts`

```js
declare module '*.vue' {
  import { DefineComponent } from 'vue'
  const component: DefineComponent<{}, {}, any>
  export default component
}
```
