#### useRef

Un hook para que el componente tenga informacion que se pueda actualizar pero que esta no desencadene nuevos renderizados.

`const ref = useRef(0);`

Para usar el valor de la referencia se puede usar la promiedad de __current__ asi como actualizar su valor.

`ref.current = ref.current + 1;`

Este hook es utilizado para hacer referencias al DOM y asi poder modificar y/o usar sus propiedades.

`<video ref={ref} src={src} loop playsInline />`
`ref.current.play()` o `ref.current.pause()`

Haciendo una comparacion con variables de estado.

| las refs | las refs |
| ----------- | ----------- |
| `useRef(initialValue)` devuelve `{ current: initialValue }` | `useState(initialValue)` devuelve el valor actual de una variable de estado y una funcion asignadora del estado (`[value, setValue]`) |
| No desencadena un rerenderizado cuando lo cambias. | Desencadena un rerenderizado cuando lo cambias. |
| Mutable: puedes modificar y actualizar el valor de __current__ fuera del proceso de renderizado. | \"Immutable\": necesitas usar la funcion asignadora del estado para modificar variables de estado para poner en cola un rerenderizado. |
| No deberias leer (o escribir) el valor de __current__ durante el renderizado. | Puedes leer el estado en cualquier momento. Sin embargo, cada renderizado tiene su propia __instantanea__ del estado que no cambia. |

Usos comunes de las referencias.

- Almacenar identificadores de timeouts
- Almacenar y manipular elementos del DOM, que cubrimos en la siguiente pagina
- Almacenar otros objetos que no son necesarios para calcular el JSX.

Buenas practicas:

- __Trata a las refs como una puerta de escape.__ Las refs son utiles cuando trabajas con sistemas externos o APIs del navegador. Si mucho de la logica de tu aplicacion y del flujo de los datos depende de las refs, es posible que quieras reconsiderar tu enfoque.
- __No leas o escribas ref.current durante el renderizado.__ Si se necesita alguna informacion durante el renderizado, usa en su lugar el estado. Como React no sabe cuando ref.current cambia, incluso leerlo mientras se renderiza hace que el comportamiento de tu componente sea dificil de predecir. (La unica excepcion a esto es codigo como if (!ref.current) ref.current = new Thing() que solo asigna la ref una vez durante el renderizado inicial).


#### useEffect

Este hook te permiten \"salir\" de React y sincronizar tus componentes con algun sistema externo. Su uso principal es:

- No necesitas Efectos para transformar los datos para el renderizado.
- No necesitas Efectos para manejar eventos de usuario.

En otros casos se deben de valorar el uso de este elemento, analizar el problema para encontrar una solucion alterna. Las dependencias innecesarias pueden causar que tu Efecto se ejecute frecuentemente, incluso llegar a crear un ciclo infinito.