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

A las rutas se les puede asignar nombre para hacer referencia a ellas, para ello
se usa la funcion _name_, como se puede ver a continuacion:

```
Route::get(
    '/user/profile',
    [UserProfileController::class, 'show']
)->name('profile');
```

En cuanto su uso se puede usar al llamar a un redirect.

```
// Generating URLs...
$url = route('profile');
 
// Generating Redirects...
return redirect()->route('profile');
 
return to_route('profile');
```

En caso de que la url necesite un parametro se puede hacer el redirect de la
siguiente forma:

```
Route::get('/user/{id}/profile', function (string $id) {
    // ...
})->name('profile');
 
$url = route('profile', ['id' => 1]);
```

Si solo necesitas generar la url se puede hacer de la siguiente fomra.

`$url = route('profile', ['id' => 1, 'photos' => 'yes']);`

#### Middleware

Puedes usar middleware para agrupar rutas.

```
Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Uses first & second middleware...
    });
 
    Route::get('/user/profile', function () {
        // Uses first & second middleware...
    });
});
```

#### Controllers

Se pueden usar la funcion de _controller_ para agrupar varias rutas que estaran
usando funciones del controlador.

```
use App\Http\Controllers\OrderController;
 
Route::controller(OrderController::class)->group(function () {
    Route::get('/orders/{id}', 'show');
    Route::post('/orders', 'store');
});
```

#### Subdomain

La funcion de _domain_ para gestionar las url de ese subdominio.

```
Route::domain('{account}.example.com')->group(function () {
    Route::get('user/{id}', function (string $account, string $id) {
        // ...
    });
});
```

#### Prefix

Se puede agrupar rutas y agregar un prefix a todas ellas.

```
Route::prefix('admin')->group(function () {
    Route::get('/users', function () {
        // Matches The "/admin/users" URL
    });
});
```

#### Ruta 404

En caso de que necesites tener un comportamiento diferente al predeterminado,
puedes modificar esta ruta mediante la siguiente funcion.

```
Route::fallback(function () {
    // ...
});
```

#### Formularios

Para los formularios se deben de consiferar estos dos parametros para que las
peticiones sean validas al mandarse las peticiones.

```
<form action="/example" method="POST">
    @method('PUT')
    @csrf
</form>
```

#### Current Route

Para obtener informacion de la ruta actual se puede hacer de la siguiente
manera.

```
use Illuminate\Support\Facades\Route;
 
$route = Route::current(); // Illuminate\Routing\Route
$name = Route::currentRouteName(); // string
$action = Route::currentRouteAction(); // string
```

### Middleware

Para crear un middle desde artisan es desde el siguiente comando.

`php artisan make:middleware EnsureTokenIsValid`

Donde uno de sus usos es validar token, como un ejemplo este codigo para enter
el funcionamiento de los _middleware_ en _Laravel_.

```
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

Para aplicar un _middleware_ a una ruta se hace de la siguiente forma.

```
use App\Http\Middleware\Authenticate;
 
Route::get('/profile', function () {
    // ...
})->middleware(Authenticate::class);
```

En caso de necesitar asignar multiples _middlewares_ a una ruta se puede ser
con el uso de un array como se ve a contiunuacion.

```
Route::get('/', function () {
    // ...
})->middleware([First::class, Second::class]);
```

### Controllers

Para crear un controlador usando artisan se debe ocupar el siguiente comando:

`php artisan make:controller UserController`

El codigo que se muestra a continuacion tiene uno de las implementaciones mas
sencillas y comunes que se requieresn.

```
<?php
 
namespace App\Http\Controllers;
 
use App\Models\User;
use Illuminate\View\View;
 
class UserController extends Controller
{
    /**
     * Show the profile for a given user.
     */
    public function show(string $id): View
    {
        return view('user.profile', [
            'user' => User::findOrFail($id)
        ]);
    }
}
```

Cuando una controlador va ser utilizado por una ruta, esta debe indicar la
funcion que va utilizar al momento de ser declara.

```
use App\Http\Controllers\UserController;
 
Route::get('/user/{id}', [UserController::class, 'show']);
```

#### Middleware

Normalmente los _middleware_ se definen en las rutas, como se puede ver a
continuacion.

`Route::get('profile', [UserController::class, 'show'])->middleware('auth');`

en caso de que necesites hacerlo desde el controlador, esto lo puedes hacer
desde el *__construct*.

```
class UserController extends Controller
{
    /**
     * Instantiate a new controller instance.
     */
    public function __construct()
    {
        $this->middleware('auth');
        $this->middleware('log')->only('index');
        $this->middleware('subscribed')->except('store');
    }
}
```