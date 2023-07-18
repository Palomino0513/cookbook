#### UseRef

Un hook para que el componente tenga informacion que se pueda actualizar pero que esta no desencadene nuevos renderizados.

`const ref = useRef(0);`

Para usar el valor de la referencia se puede usar la promiedad de __current__ asi como actualizar su valor.

`ref.current = ref.current + 1;`

Este hook es utilizado para hacer referencias al DOM y asi poder modificar y/o usar sus propiedades.

`<video ref={ref} src={src} loop playsInline />`
`ref.current.play()` o `ref.current.pause()`

