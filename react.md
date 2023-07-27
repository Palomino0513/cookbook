# React Js Cookbook
### Modificacion de array como _State_

| Accion     | Evita (muta el array)       | Preferido (retorna un nuevo array)       | Ejemplo     |
|------------|-----------------------------|------------------------------------------|-------------|
| Añadir     | push, unshift               | concat, [...arr] operador de propagación | `Pendiente` |
| Eliminar   | pop, shift, splice          | filter, slice                            | `Pendiente` |
| Reemplazar | splice, arr[i] = ... asigna | map                                      | `Pendiente` |
| Ordenar    | reverse, sort               | copia el array primero                   | `Pendiente` |

### useRef

Un hook para que el componente tenga informacion que se pueda actualizar pero
que esta no desencadene nuevos renderizados.

`const ref = useRef(0);`

Para usar el valor de la referencia se puede usar la promiedad de __current__
asi como actualizar su valor.

`ref.current = ref.current + 1;`

Este hook es utilizado para hacer referencias al DOM y asi poder modificar y/o
usar sus propiedades.

`<video ref={ref} src={src} loop playsInline />`
`ref.current.play()` o `ref.current.pause()`

Haciendo una comparacion con variables de estado.

| las refs                                                                                         | las refs                                                                                                                               |
|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `useRef(initialValue)` devuelve `{ current: initialValue }`                                      | `useState(initialValue)` devuelve el valor actual de una variable de estado y una funcion asignadora del estado (`[value, setValue]`)  |
| No desencadena un rerenderizado cuando lo cambias.                                               | Desencadena un rerenderizado cuando lo cambias.                                                                                        |
| Mutable: puedes modificar y actualizar el valor de __current__ fuera del proceso de renderizado. | \"Immutable\": necesitas usar la funcion asignadora del estado para modificar variables de estado para poner en cola un rerenderizado. |
| No deberias leer (o escribir) el valor de __current__ durante el renderizado.                    | Puedes leer el estado en cualquier momento. Sin embargo, cada renderizado tiene su propia __instantanea__ del estado que no cambia.    |

Usos comunes de las referencias.

- Almacenar identificadores de timeouts
- Almacenar y manipular elementos del DOM, que cubrimos en la siguiente pagina
- Almacenar otros objetos que no son necesarios para calcular el JSX.

Buenas practicas:

- __Trata a las refs como una puerta de escape.__ Las refs son utiles cuando
  trabajas con sistemas externos o APIs del navegador. Si mucho de la logica
  de tu aplicacion y del flujo de los datos depende de las refs, es posible
  que quieras reconsiderar tu enfoque.
- __No leas o escribas ref.current durante el renderizado.__ Si se necesita
  alguna informacion durante el renderizado, usa en su lugar el estado. Como
  React no sabe cuando ref.current cambia, incluso leerlo mientras se renderiza
  hace que el comportamiento de tu componente sea dificil de predecir. (La unica
  excepcion a esto es codigo como `if (!ref.current) ref.current = new Thing()`
  que solo asigna la ref una vez durante el renderizado inicial).

### useEffect

Este hook te permiten \"salir\" de React y sincronizar tus componentes con algun
sistema externo. Su uso principal es:

- No necesitas Efectos para transformar los datos para el renderizado.
- No necesitas Efectos para manejar eventos de usuario.

En otros casos se deben de valorar el uso de este elemento, analizar el problema
para encontrar una solucion alterna. Las dependencias innecesarias pueden causar
que tu Efecto se ejecute frecuentemente, incluso llegar a crear un ciclo
infinito.

Para declarar un efecto en tu componente, importa el Hook useEffect desde React:

```
import { useEffect } from 'react';
function MyComponent() {
  useEffect(() => {
    // El codigo aqui se ejecutara despues de \*cada\* renderizado
  });
  return <div />;
}
```

Recordemos que estos hook hacen que \"salgas\" de React, por lo cual debes de
tener estas consideracion para su uso:

- A veces, es lento. Sincronizar con un sistema externo no siempre es
  instantaneo, por lo que es posible que desees evitar hacerlo a menos que sea
  necesario.
- A veces, esta mal. Por ejemplo, no quieres desencadenar una animacion de
  desvanecimiento en un componente en cada pulsacion de tecla. La animacion solo
  se debe reproducir cuando el componente aparece por primera vez.

Tambien este hook tiene diferentes comportamientos dependiento la configuracion
que uses:

```
useEffect(() => {
  // Esto se ejecuta despues de cada renderizado
});
```

```
useEffect(() => {
  // Esto solo se ejecuta en el montaje (cuando el componente aparece)
}, []);
```

```
useEffect(() => {
  // Esto se ejecuta en el montaje \*y tambien\* si a o b han cambiado desde el ultimo renderizado
}, [a, b]);
```

Lo mas recomendable es siempre hacer una funcion para desmontar y desvicular
eventos o detener procesos que se iniciaron en el montaje del componente.

### useEffectEvent

Utiliza un Hook especial llamado __useEffectEvent__ para extraer esta logica no
reactiva de su Efecto:

```
import { useEffect, useEffectEvent } from 'react';

function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Conectado!', theme);
  });
```

Ahora puedes llamar al Evento de Efecto onConnected desde dentro de tu Efecto:

```
function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Conectado!', theme);
  });

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      onConnected();
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
  // ...
```

Los Eventos de Efecto no son reactivos, deben ser omitidos de las dependencias y
estos tienen un uso muy limitado:

- Llamalos solo desde dentro Efectos.
- Nunca los pases a otros componentes o Hooks.

### Hooks personalizados

Los hooks personalizados son para componentes o funciones que se estaran
reutiliznado. Cuando se crea un hook personalidado por convencion su nombre debe
de empezar con __use__ seguido del nombre que se le quiera poner al hook en
formato _upper cammel case_. Ejemplo:

```
import { useState } from 'react';

export function useFormInput(initialValue) {
  const [value, setValue] = useState(initialValue);

  function handleChange(e) {
    setValue(e.target.value);
  }

  const inputProps = {
    value: value,
    onChange: handleChange
  };

  return inputProps;
}
```

Los Hooks personalizados permiten compartir la lógica con estado pero no el
estado en sí. Cada invocación al Hook es completamente independiente de
cualquier otra invocación al mismo Hook.

Antes de pensar en hacer un hook
valdria la pena hacer una breve busqueda para averiguar si no existe ya un hook
para la necesidad que queremos cubrir ya que la comunidad es muy activa y es
posible que ya exista un hook para nuestra necesidad.

## Recomendaciones

React requiere experiancia bastante experiencia para poder dominarlo, debido a
que se requiere hacerlo a la forma en que React recomienda de lo contrario el
performance de la aplicacion se veria afectado, ademas de algunos
comportamientos raros y bugs aparecerian en la aplicacion; por lo tanto se en
lista una serie de recomedaciones a tener en cuenta en el momento que estemos
creando nuestros componentes en React.

- Si el código en tu Efecto debe ejecutarse como respuesta a una interacción
  específica, mueve el código a un controlador de evento.
- Si partes diferentes de tu Efecto deberían volverse a ejecutar por diferentes
  razones, divídelo en diferentes Efectos.
- Si quieres actualizar un estado basado en el estado anterior, pasa una función
  actualizadora.
- Si quieres leer el último valor sin «reaccionar» a él, extrae un Evento de
  Efecto de tu Efecto.
- En JavaScript, los objetos y funciones se consideran diferentes si se crean en
  momentos diferentes.
- Intenta evitar objetos y funciones como dependencias. Muévelos fuera del
  componente o dentro del Efecto.