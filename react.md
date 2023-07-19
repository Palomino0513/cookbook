#### useRef

Un hook para que el componente tenga informacion que se pueda actualizar pero que esta no desencadene nuevos renderizados.

`const ref = useRef(0);`

Para usar el valor de la referencia se puede usar la promiedad de __current__ asi como actualizar su valor.

`ref.current = ref.current + 1;`

Este hook es utilizado para hacer referencias al DOM y asi poder modificar y/o usar sus propiedades.

`<video ref={ref} src={src} loop playsInline />`
`ref.current.play()` o `ref.current.pause()`

#### useEffect

Este hook te permiten \"salir\" de React y sincronizar tus componentes con algun sistema externo. Su uso principal es:

- No necesitas Efectos para transformar los datos para el renderizado.
- No necesitas Efectos para manejar eventos de usuario.

En otros casos se deben de valorar el uso de este elemento, analizar el problema para encontrar una solucion alterna. Las dependencias innecesarias pueden causar que tu Efecto se ejecute frecuentemente, incluso llegar a crear un ciclo infinito.