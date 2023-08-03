# Laravel Cookbook
## Instalacions

Para crear un proyecto se puede hacer con el siguiente comando:

`curl -s "https://laravel.build/example-app" | bash`

Para iniciar el proyecto se debe ejecutar el siguiente comando:

`./vendor/bin/sail up`

Para detener los servicios de __sail__ se hace con el comando:

`./vendor/bin/sail donw`

Para hacer mas practico el uso de __sail__ puedes crear un alias editando el
archivo ".bashrc" y agregando la siguiente linea:

`alias sail="./vendor/bin/sail"`

## Configuracion
### Laravel Breeze

Es la implementacion recomendada de Laravel para el manejo de funcionalidades de
autenticacion donde se incluye logeo, registro, cambio de password, verificacion
y confirmacion via email. Tambien integra las funciones de pagina de perfil
donde se puede actualizar nombre, email y contrasena.

#### Instalacion

Antes de instarlar _Breeze_ se recomienda haber ejecutado las migraciones que se
tengan en el proyecto. El comando para instalar es el siguiente:

`composer require laravel/breeze --dev`

Despues de la instalacion se necesita ejecutar los siguientes comandos para
terminar de crear los elementos necesarios para el funcionamiento de _Breeze_.

```
php artisan breeze:install blade
php artisan migrate
npm install
npm run dev
```

En este caso se tiene ejecuta la instalacion para _blade_ pero _Breeze_ tiene
instalaciones para _frameworks_ como _React Js_ o _Vue Js_, para lo cual se
recomienda revizar la documentacion para hacer dichas instalaciones.

## Conceptos
### Rutas

Para declarar una ruta se necesita la ruta de entrada, el controlador que va a
resolver la peticion, un ejemplo de esto es el siguiente codigo:

```
use App\Http\Controllers\UserController;
 
Route::get('/user', [UserController::class, 'index']);
```

Tambien puedes usar diferentes tipos de metodos para resolver peticiones.

```
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

Puede se puede tener varios metodos que apunten a controler o funcion.

```
Route::match(['get', 'post'], '/', function () {
    // ...
});
```

Tambien puedes responder a todas las peticiones a una ruta de la siguiente
manera.

```
Route::any('/', function () {
    // ...
});
```

Se pueden hacer redireccionar peticiones.

`Route::redirect('/here', '/there');`

A las redirecciones se les puede agregar un tercer parametro para definir el
estatus de la redirecion o incluso usar _permanentRedirect_ en vez de _redirect_
para redireccionar siempre con estatus 301.

#### View Routes

En caso de solo necesitar regresar una vista y tal vez agregar unos parametros
se puede hacer de la sigueinte forma.

`Route::view('/welcome', 'welcome', ['name' => 'Taylor']);`

#### Condicionales

Existe el condicional _where_ para agregar condificones al momento de resolver
las rutas que estamos definiendo.

```
Route::get('/user/{name}', function (string $name) {
    // ...
})->where('name', '[A-Za-z]+');
 
Route::get('/user/{id}', function (string $id) {
    // ...
})->where('id', '[0-9]+');
 
Route::get('/user/{id}/{name}', function (string $id, string $name) {
    // ...
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```

ademas existen funciones que tienen validaciones mas especificas que en la
funcion _where_ y que son de uso comun.

```
Route::get('/user/{id}/{name}', function (string $id, string $name) {
    // ...
})->whereNumber('id')->whereAlpha('name');
 
Route::get('/user/{name}', function (string $name) {
    // ...
})->whereAlphaNumeric('name');
 
Route::get('/user/{id}', function (string $id) {
    // ...
})->whereUuid('id');
 
Route::get('/user/{id}', function (string $id) {
    //
})->whereUlid('id');
 
Route::get('/category/{category}', function (string $category) {
    // ...
})->whereIn('category', ['movie', 'song', 'painting']);
```

#### Nombres en las Rutas

