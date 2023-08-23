# Magento 2 Knockout JS

Knockout es una libreria basada en el patron de diseno Model-View-View Model
(MVVM), con el objetivo de facilitar el area de frontend en magento, teniendo
como principales tareas:

- Observables e inyeccion y seguimiento de dependencias.
- Definicion de Eventos (Bindings).
- Manejo de sistema de plantillas.

## Conceptos
### Observables

Un observable es un elemento el cual almacena un valor y ademas provee de una
funcion para asginar su valor y obtenerlo, mas conocidas como _setters_ y
_getters_. La particularidad de estos observables es que en el momento en que
el valor se cambia, este actualiza todos los elementos que esten utilizando su
valor.

Ejemplo de como se declara un json con elelemtos observables:
```
var myViewModel = {
    personName: ko.observable('Bob'),
    personAge: ko.observable(123)
};
```
Para al acceder al valor del observable se usa la funcion _getter_:
```
myViewModel.personName() // return 'Bob'
myViewModel.personAge() // return 123
```
Para actualizar un valor se realiza pasando un argumento al _setter_:
```
myViewModel.personName('Mary')
```
Un caso especial es que en objetos _JSON_ se pueden encadenar las funciones
_setter_, asi podiendo actualizar varios observables del mismo _JSON_.
```
myViewModel
    .personName('Mary')
    .personAge(50)
```

## Lenguaje