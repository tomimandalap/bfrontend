# Web Kedai Kopi

Project membuat web pemesanan yang saya beri nama kedai kopi cah bagus yang sedikit lebih disempurnakan secara tampilan maupun fungsinya dari project [html dan css], Project ini dipermisalkan sebagai alat untuk transaksi panjualan di kedai kopi cah bagus yang terhubung dengan database REST API atau terintegrasi dengan project [beginner-backend]. Materi presentasi klik [disini]

Project ini menggunakan beberapa freamwork silahkan install didalam kode editor seperti VS Code atau sejenisnya:
### [Vue CLI]
```
# With npm
npm install -g @vue/cli
```
### [bootstrap vue]
```
# With npm
npm install vue bootstrap bootstrap-vue
```
### Kemudian silahkan register BootstrapVue ke dalam entri point app (app.js atau main.js).
```
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// Import Bootstrap an BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

// Make BootstrapVue available throughout your project
Vue.use(BootstrapVue)
// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)
```
### [axios]
```
npm install axios
```

---

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

---

### program frontend

![f](https://user-images.githubusercontent.com/43200304/104850877-423e3580-5924-11eb-80c5-aff33d7fe4c3.PNG)


### program backend

![g](https://user-images.githubusercontent.com/43200304/104850878-42d6cc00-5924-11eb-98b2-440fb7447928.PNG)


### Tampilan Web

![a](https://user-images.githubusercontent.com/43200304/104850879-436f6280-5924-11eb-99e8-4b11789000ce.PNG)


### Tampilan Checkout

![b](https://user-images.githubusercontent.com/43200304/104850880-44a08f80-5924-11eb-8288-0eeac705ecdf.PNG)


### Tampilan Add Items

![c](https://user-images.githubusercontent.com/43200304/104850882-45d1bc80-5924-11eb-842b-b3f2773b1cb2.PNG)


### Tampilan Edit dan Delete

![d](https://user-images.githubusercontent.com/43200304/104850883-4702e980-5924-11eb-91fb-39257f97c0fc.PNG)


### Tampilan Tabel History

![e](https://user-images.githubusercontent.com/43200304/104850876-3fdbdb80-5924-11eb-85e0-a73406ed695b.PNG)


[html dan css]: https://github.com/tomimandalap/deliveryfastfood
[beginner-backend]: https://github.com/tomimandalap/beginer_backend
[Vue CLI]: https://cli.vuejs.org/guide/installation.html
[bootstrap vue]: https://bootstrap-vue.org/docs
[axios]: https://www.npmjs.com/package/axios
[disini]: https://docs.google.com/presentation/d/15-JHHpY_ve6x5-j08-YjGzKqtuREV19oPcXOxFjLQxU/edit?usp=sharing
