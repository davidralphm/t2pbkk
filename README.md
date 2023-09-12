# t2pbkk

## Membuat route
Untuk membuat route baru, kita dapat menggunakan fungsi dari method yang akan digunakan untuk route tersebut. Misal, jika kita ingin membuat route untuk request method `GET`, kita dapat menggunakan fungsi `app.get()`. Di fungsi ini, kita dapat menambahkan path yang akan digunakan untuk route yang kita buat, dan fungsi yang akan dipanggil ketika kita melakukan GET request ke path tersebut. Berikut adalah contoh kode untuk membuat route baru.

``` js
var express = require('express');
var app = express();

app.get('/test', function(req, res) {
  res.send('Hello, world!')
});

app.listen(3000);
```

## Membuat controller
Untuk membuat controller, pertama kita sebagai contoh dapat membuat sebuah file baru bernama `routes/products.js`. Di dalamnya, kita dapat menambahkan kode berikut. Kita menambahkan nama - nama dari route dan path untuk route yang ingin kita buat.
``` js
const express = require('express')
const router = express.Router()

const  { 
    getProducts,
    getProduct,
    createProduct,
    updateProduct,
    deleteProduct 
} = require('../controllers/products.js')

router.get('/', getProducts)

router.get('/:productID', getProduct)

router.post('/', createProduct) 

router.put('/:productID', updateProduct) 

router.delete('/:productID', deleteProduct)

module.exports = router
```

Selanjutnya, kita dapat membuat file baru bernama `controllers/products.js`. Di dalam file ini, kita akan membuat implementasi untuk setiap route yang sudah kita buat di dalam file `routes/products.js`. Berikut adalah kodenya.
``` js
const products = require('../data.js')

const getProducts = ((req, res) => {
    res.json(products)
})

const getProduct = ((req, res) => {
    const id = Number(req.params.productID)
    const product = products.find(product => product.id === id)

        if (!product) {
        return res.status(404).send('Product not found')
    }
    res.json(product)
})

const createProduct = ((req, res) => {
    const newProduct = {
        id: products.length + 1,
        name: req.body.name,
        price: req.body.price
    }
    products.push(newProduct)
    res.status(201).json(newProduct)
})

const updateProduct = ((req, res) => {
    const id = Number(req.params.productID)
    const index = products.findIndex(product => product.id === id)
    const updatedProduct = {
        id: products[index].id,
        name: req.body.name,
        price: req.body.price
    }

    products[index] = updatedProduct
    res.status(200).json('Product updated')
})

const deleteProduct = ((req, res) => {
    const id = Number(req.params.productID)
    const index = products.findIndex(product => product.id === id)
    products.splice(index,1)
    res.status(200).json('Product deleted')
})

module.exports = {
    getProducts,
    getProduct,
    createProduct,
    updateProduct,
    deleteProduct
}
```

Setelah itu, agar kita dapat menggunakan route dan controller yang sudah kita buat, kita dapat menambahkan kode berikut dalam file `main.js` kita.
``` js
const express = require('express')
const app = express()
const products = require('./data.js')
const products_routes = require('./routes/products.js')

app.listen(5000, () => {
    console.log('server is listening on port 5000')
})

app.use(express.json())
app.use('/api/products', products_routes)
```

## Membuat model
Database yang sering digunakan dengan ExpressJS adalah MongoDB, yang merupakan database NoSQL. Agar bisa menggunakan MongoDB dengan ExpressJS, diperlukan sebuah Client API. Salah satu Client API yang sering digunakan adalah Mongoose. Untuk menginstall Mongoose, dapat menjalankan perintah berikut.
`npm install --save mongoose`

Setelah itu, kita harus membuat sebuah database. Untuk membuat database baru, pertama kita harus membuka terminal dan mengetikkan perintah `mongo`. Ini akan membuka shell Mongo, dan di dalam shellnya kita dapat mengetikkan perintah berikut untuk membuat database baru yang bernama `test_db`.
`use test_db`

Agar bisa menggunakan Mongoose, kita perlu menambahkan kode berikut.
``` js
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test_db');
```

Setelah kita terhubung dengan database yang telah kita buat, kita dapat membuat sebuah model baru. Berikut ada contoh kode untuk membuat model. Kode tersebut harus ditambahkan sebelum kita membuat route.
``` js
var personSchema = mongoose.Schema({
   name: String,
   age: Number,
   nationality: String
});
var Person = mongoose.model("Person", personSchema);
```
Kode tersebut akan membuat model baru yang bernama `Person`.

## Membuat service


## Membuat middleware

Untuk membuat middleware, kita dapat menggunakan fungsi `use` pada app yang telah kita buat. Dalam fungsi tersebut, kita menambahkan fungsi dari middleware yang kita buat. Berikut adalah contoh membuat middleware sederhana.

``` js
var express = require('express');
var app = express();

app.use(function(req, res) {
  console.log('Contoh middleware untuk logging request');

  next();
});
```

## Menggunakan template dan views

Template engine yang sering digunakan dengan Express adalah Pug. Pug dapat diinstall dengan menjalankan command berikut.
`npm install --save pug`

Setelah menginstall Pug, kita dapat menambahkan kode berikut agar dapat menggunakannya.
``` js
app.set('view engine', 'pug');
app.set('views','./views');
```

Setelah itu, kita membuat directory dengan nama `views`. Di dalamnya, kita membuat file bernama `test.pug`. Di dalam file tersebut, kita dapat menambahkan kode berikut.
``` pug
doctype html
html
   head
      title = "Hello World"
   body
      p.greetings#people Hello World!
```

Untuk merender template tersebut, kita dapat menambahkan kode berikut dalam app kita.
``` js
app.get('/test_template', function(req, res){
   res.render('test');
});
```

## Referensi

- https://www.tutorialspoint.com/expressjs/index.htm
- https://www.tutorialspoint.com/expressjs/expressjs_templating.htm
- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes
- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/mongoose
- https://www.tutorialspoint.com/expressjs/expressjs_database.htm
- https://dev.to/ericchapman/nodejs-express-part-5-routes-and-controllers-55d3
