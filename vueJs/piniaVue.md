# Source

source bisa dilihat di :

```bash
https://pinia.vuejs.org/getting-started.html
```

# Instalasi

instalasi via `npm` :

```bash
npm install pinia
```

# Setup Pinia

set up pada file `main.js` atau `main.ts` :

```javascript
import { createPinia } from "pinia";

const pinia = createPinia();

createApp().use(pinia).mount("#app");
```

# Folder Store

buat satu folder berisi file store, beri nama sesuai kebutuhan , disini saya menggunakan contoh Store Login :

```bash
├── store
│   └── storeLogin.js
```

lalu setup dengan minimal code seperti ini :

```javascript
import { defineStore } from "pinia";
export const useExampleStore = defineStore("example", {
  state: () => ({
    items: [],
  }),
  getters: {
    functionName: (state) => {},
  },
  actions: {
    namaFunction(data) {},
  },
});
```

penjelasan :
`state` berfungsi mengatur state awal sebelum ada interaksi :

```javascript
state: () => ({
    items: [],
}),
```

`getters` berfungsi mengatur state yang akan di akses dari store, berisi fungsi `getter` :

```javascript
getters: {
    functionName: (state) => {},
},
```

`actions` berfungsi mengatur state yang akan di akses dari store, berisi fungsi `action` :

```javascript
actions: {
    namaFunction(data) {},
},
```

# Use Pinia
