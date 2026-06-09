```bash
https://flowbite.com/docs/getting-started/quickstart/
```

# Instalasi

```bash
npm install flowbite
```

# Konfigurasi

import di file css utama `resources/css/app.css`

```css
@import "flowbite/src/themes/default";
@plugin "flowbite/plugin";
```

di dalam `resources/app.js` tambahkan ini :

```js
import { initFlowbite } from "flowbite";
```

dan

```js
nextTick(() => {
  if (typeof window !== "undefined") {
    initFlowbite();
  }
});
```

dan

```js
router.on("finish", () => {
  initFlowbite();
});
```

atau lebih lengkapnya :

```js
import { createApp, h, nextTick, onMounted } from "vue";
import { createInertiaApp, router } from "@inertiajs/vue3";
import { initFlowbite } from "flowbite";

createInertiaApp({
  resolve: (name) => {
    const pages = import.meta.glob("./Pages/**/*.vue");
    return pages[`./Pages/${name}.vue`]();
  },
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el);
    nextTick(() => {
      if (typeof window !== "undefined") {
        initFlowbite();
      }
    });
  },
});

router.on("finish", () => {
  initFlowbite();
});
```

#
