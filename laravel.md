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




