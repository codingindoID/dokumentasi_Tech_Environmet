# Instalasi

source :

```bash
https://tanstack.com/
```

install :

```bash
npm i @tanstack/vue-query
```

# Daftarkan pada main.js

daftarkan pad main.js :

```js
import { createApp } from "vue";
import App from "./App.vue";
import { VueQueryPlugin } from "@tanstack/vue-query";

createApp(App).use(VueQueryPlugin).mount("#app");
```

# SetUp Folder API dan Query

buat 2 folder untuk menampung API dan menampung query Fetch :
misal saya punya endPoint API product

```css
src/
 ├─ api/
 │   ├─ product.api.js
 │   ├─ cart.api.js
 │   └─ auth.api.js
 ├─ queries/
 │   └─ useProducts.js
 │
 └─ pages/
     └─ Products.vue
```

```bash
src/api/product.api.js
src/queries/useProduct.js
```

api berisi endpoint , idealnya jenis endpoint dikumpulkan berdasarkan fungsi utama :

```js
const BASE_URL = "url";
export async function fetchProducts() {
  const res = await fetch(BASE_URL);
  if (!res.ok) throw new Error("Fetch failed");
  return res.json();
}
//post
export async function postProduct() {
  const res = await fetch(`${BASE_URL}/endPoint`);
  if (!res.ok) throw new Error("Fetch failed");
  return res.json();
}
```

query untuk tampungan / eksekusi api :

```js
import { useQuery } from "@tanstack/vue-query";
import { fetchProducts } from "../api/products.api";

export function useProducts() {
  return useQuery({
    queryKey: ["products"],
    queryFn: fetchProducts,
    staleTime: 1000 * 60 * 5,
  });
}
```

# setUp Pada page

```vue
<script setup>
import { ref, reactive } from "vue";
import { useProducts } from "@/queries/useProducts";

const { data, isLoading, error } = useProducts();
const products = ref([]);
</script>

<template>
  <div class="display">
    <h3>Data Products</h3>
    <p v-if="isLoading">Loading...</p>
    <p v-if="error">Gagal memuat data</p>
    <p v-for="product in data?.products" :key="product.id">
      {{ product.title }} <br />
      ${{ product.price }} <br />
      available : {{ product.stock }}
    </p>
  </div>
</template>
```
