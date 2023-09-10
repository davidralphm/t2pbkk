# t2pbkk

## Membuat route

### Express

``` js
var express = require('express');
var app = express();

app.get('/test', function(req, res) {
  res.send('Hello, world!')
});

app.listen(3000);
```

### Laravel

``` php
Route::get('/', function() {
  return view('welcome');
});
```

### Fastify

``` js
const fastify = require('fastify')({
  logger: true
});

fastify.get('/', function(request, reply) {
  reply.send({
    hello: 'world'
  });
});

fastify.listen(3000, function(err, address) {
  if (err) {
    fastify.log.error(err);
    process.exit(1);
  }
});
```

## Middleware

### Express

``` js
var express = require('express');
var app = express();

app.use(function(req, res) {
  console.log('Contoh middleware untuk logging request');

  next();
});
```

### Laravel

#### Membuat middleware
Untuk membuat middleware baru, dapat menggunakan command berikut:
`php artisan make:middleware TestMiddleware`

#### Register middleware
Untuk menggunakan sebuah middleware, kita harus meregisternya terlebih dahulu. Dalam Laravel terdapat dua macam middleware, yaitu Global Middleware dan Route Middleware. Global Middleware akan dijalankan untuk setiap HTTP request, sedangkan Route Middleware hanya dijalankan pada route - route tertentu.

Meregister middleware dapat dilakukan dengan mengedit file `app/Http/Kernel.php`. Dalam file ini terdapat dua properties, yaitu `$middleware` dan `$routeMiddleware`. Property `$middleware` digunakan untuk meregister Global Middleware dan property `$routeMiddleware` digunakan untuk meregister Route Middleware.

``` php

```

### Fastify

## Templating
