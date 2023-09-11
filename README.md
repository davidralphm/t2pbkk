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
`php artisan make:middleware EnsureTokenIsValid`
Menjalankan command tersebut akan membuat file bernama EnsureTokenIsValid di dalam direktori `app/Http/Middleware`.

Berikut adalah isi dari file middleware EnsureTokenIsValid.php:

``` php
<?php
 
namespace App\Http\Middleware;
 
use Closure;
use Illuminate\Http\Request;
use Symfony\Component\HttpFoundation\Response;
 
class EnsureTokenIsValid
{
    /**
     * Handle an incoming request.
     *
     * @param  \Closure(\Illuminate\Http\Request): (\Symfony\Component\HttpFoundation\Response)  $next
     */
    public function handle(Request $request, Closure $next): Response
    {
        if ($request->input('token') !== 'my-secret-token') {
            return redirect('home');
        }
 
        return $next($request);
    }
}
```

#### Register middleware
Dalam Laravel terdapat dua macam middleware, yaitu Global Middleware dan Route Middleware. Global Middleware akan dijalankan untuk setiap HTTP request, sedangkan Route Middleware hanya akan dijalankan untuk route - route tertentu saja.

Untuk meregister Global Middleware dapat dilakukan dengan mengedit file `app/Http/Kernel.php` dan menambahkan middlewarenya ke dalam property `$middleware`.

Untuk meregister middleware pada sebuah route tertentu, kita dapat menggunakan method `middleware` ketika membuat sebuah route.

``` php
use App\Http\Middleware\Authenticate;
 
Route::get('/profile', function () {
    // ...
})->middleware(Authenticate::class);
```

### Fastify
Untuk menggunakan middleware dalam Fastify, kita harus menginstall plugin eksternal seperti `@fastify/express` atau `@fastify/middie`. Berikut adalah contoh menggunakan middleware `cookie-parser`.

``` js
import Fastify from "fastify";
import middie from "@fastify/middie";
import cookieParser from "cookie-parser";
 
const fastify = Fastify({
  logger: true,
});
 
await fastify.register(middie);
 
fastify.use(cookieParser());
 
fastify.get("/", function (request, reply) {
  console.log("Cookies: ", request.raw.cookies);
  reply.send("Hello world!");
});
 
const port = process.env.PORT || 3000;
 
fastify.listen({ port }, function (err, address) {
  if (err) {
    fastify.log.error(err);
    process.exit(1);
  }
 
  fastify.log.info(`Fastify is listening on port: ${address}`);
});
```

## Templating

## Referensi

- https://www.tutorialspoint.com/expressjs/index.htm
- https://medium.com/@vishalims095/express-vs-fastify-9a4d052c28e1
- https://www.tutorialspoint.com/laravel/index.htm
- https://fastify.dev/docs/v3.29.x/Reference/
- https://cleancommit.io/blog/laravel-vs-express-which-framework-is-more-powerful/
- https://laravel.com/docs/10.x/middleware
- https://blog.appsignal.com/2023/05/24/advanced-fastify-hooks-middleware-and-decorators.html
