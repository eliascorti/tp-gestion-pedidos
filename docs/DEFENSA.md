# Guía para estudiar y defender el TP

No alcanza con ejecutar la app: todos los integrantes tienen que entender el recorrido completo.

## Orden sugerido de lectura

1. `src/types/`: qué devuelve cada entidad de la API. Un pedido tiene IDs y cantidades, no productos completos.
2. `src/services/api.ts` y `services/pedidos.ts`: cómo se arma una solicitud HTTP, cómo se comprueba `respuesta.ok` y cómo se envía JSON.
3. `src/utils/pedidos.ts`: funciones sin React ni red. Reciben datos y devuelven números. Los precios se calculan en centavos.
4. `src/context/pedidos-context.tsx`: obtiene las listas iniciales, construye mapas y comparte los cambios de la sesión.
5. `src/app/_layout.tsx` y `(tabs)/_layout.tsx`: el stack contiene las pestañas y las pantallas de detalle/formulario.
6. `src/components/formulario-pedido.tsx`: mismo formulario para alta y edición; `pedido` es opcional y precarga los valores.
7. `src/hooks/use-carga.ts`: carga, errores, reintento y limpieza al abandonar una pantalla.

## Recorrido de una operación

Al guardar, el formulario valida → el Context llama al servicio → el servicio envía POST o PUT → comprueba HTTP → el Context aplica la respuesta al estado → muestra confirmación y vuelve atrás.

La app conserva la fecha original al editar y toma la fecha actual al crear. Las fechas se muestran sin convertirlas a hora local: hacerlo convertiría la medianoche UTC del ejemplo en el día anterior en Argentina.

## Por qué hay dos tipos de datos en el Context

- Las listas iniciales vienen de GET y no cambian en el servidor.
- `cambios` contiene altas y ediciones; `null` representa una anulación local.
- `combinarPedidos` cruza las dos fuentes por ID. Un GET posterior no debe deshacer un cambio local.
- `productosPorId` y `clientesPorId` permiten resolver nombres y precios sin hacer una solicitud por tarjeta.
- La API puede repetir el ID 11 en varios POST. `idDisponible` reserva un ID distinto para cada alta de esta sesión.
- DELETE de un alta local puede responder `null` con HTTP 200. Es válido: ese registro nunca existió realmente en el servidor.

## Demo sugerida (15 minutos)

1. Mostrar pedidos, fecha, unidades y total.
2. Abrir un pedido y explicar un subtotal.
3. Tocar el cliente y abrir uno de sus pedidos.
4. Abrir catálogo y filtrar una categoría.
5. Crear un pedido; agregar dos veces el mismo producto; comprobar que hay una sola fila.
6. Intentar guardar sin cliente o con cantidad inválida. Corregir y guardar.
7. Editar cantidad y cliente. Volver y verificar el total y ambas fichas de cliente.
8. Anular con confirmación. Comprobar que desaparece del listado.
9. Explicar que al reiniciar la aplicación reaparecen los datos originales de Fake Store API.

## Cambios pequeños para practicar sin IA

- Cambiar el texto del botón cuando el total supera un importe.
- Agregar una validación de máximo 20 unidades por producto.
- Mostrar la cantidad de productos distintos en el detalle.
- Agregar una observación opcional al formulario (decidir cómo conservarla si la API no la admite).
- Cambiar un color en `constants/colores.ts` y explicar dónde se reutiliza.

## Trabajo del grupo

Completar los nombres en README, registrar prompts reales adicionales si los usan, revisar las capturas y probar en un celular con Expo Go compatible con SDK 54. Cada integrante debe hacer cambios que comprenda y registrarlos con su propia identidad Git. No fabricar autores ni un historial de participación.
