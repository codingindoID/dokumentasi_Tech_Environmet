<h1>Provide dan Inject</h1>

# Apa itu Provide / Inject?

ini digunakan untuk interaksi data antar komponent. Vue menyediakan mekanisme:

```bash
Parent
  ↓ provide
Child
  ↓ inject
GrandChild
```

untuk berbagi data atau function tanpa harus mengoper props berkali-kali. contoh :

```bash
PosLanding
├── PosHeader
├── PosProduct
└── PosPesanan
```

# Povide

Secara gampang Provide adalah Parent Komponen Menyediakan function yang dipanggil. misal :

```bash
provide("cart", cart)
provide("addToCart", addToCart)
provide("seacrh", search)
```

# Inject

Lalu Function `Provide` dipanggil di child seperti ini :

```bash
const cart = inject("cart")
const addToCart = inject("addToCart")
const seacrh = inject("seacrh")
```

# Contoh Penerapan Pada Fitur Search

parent menyediakan :

```javascript
const search = ref("");

provide("search", search);
```

lalu di Komponen yang mengandung Field Seacrh :

```javascript
const search = inject("search");
```

kemudian komponen :

```vue
<input v-model="search" />
```

dan didalam komponen yang mengandung element hasil pencarian , misal product :

```javascript
const search = inject("search");
```

dan poses filter

```javascript
const filteredProducts = computed(() => {
  return products.value.filter((p) =>
    p.name.toLowerCase().includes(search.value.toLowerCase()),
  );
});
```
