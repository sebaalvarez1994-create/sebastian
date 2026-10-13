# Valsur Logística — Panel de control

Aplicación web de una sola página (`index.html`) con tres secciones y tema claro / oscuro (botón ☀/☾ arriba a la derecha):

- **Panel Jefe**: mapa 3D isométrico con la ubicación de todos los vehículos en tiempo real, estado de cada camión (cargando, en ruta, descargando, regresando, incidencia, en taller), pedidos a bordo con su línea de tiempo y tablero de pedidos por etapa (Recibido → En preparación → Cargado → En tránsito → Entregado).
- **Depósito**: buscador de mercadería por nombre, código, lote o ubicación (ej. `B-04`). Al elegir un producto se resalta su posición en la estantería 3D (pasillo, posición y nivel) y se muestra el detalle de lotes y vencimientos.

- **Choferes**: perfiles con nombre, DNI, teléfono, categoría de licencia, vehículo asignado, vencimientos (licencia de conducir, LiNTI, psicofísico, cargas peligrosas, ART, libreta sanitaria) y habilitaciones. Se pueden agregar, editar y eliminar. Los vencidos o próximos a vencer (30 días) se marcan en rojo / amarillo, y el Panel Jefe avisa si el chofer de un camión tiene documentación vencida.

## Cargar inventario desde Excel

En **Depósito → Importar Excel** (acepta `.xlsx`, `.xls` o `.csv`). Se lee la primera hoja con este formato (la fila de títulos es opcional):

| Columna | Dato | Ejemplo |
|---|---|---|
| A | Nombre del producto | Aceite de girasol 1,5 L |
| B | Cantidad | 120 |
| C | Fecha de vencimiento (puede quedar vacía) | 31/12/2026 |
| D | Zona en el mapa: pasillo `A`–`F`, opcionalmente posición `01`–`10` y nivel `1`–`4` | `B`, `B-04` o `B-04-2` |

Antes de importar se muestra una vista previa con las filas que tienen errores, y se elige entre **sumar** al inventario actual o **reemplazarlo**. Si la posición pedida está ocupada, el producto se ubica en la siguiente libre del mismo pasillo y se avisa. El botón **Plantilla** descarga un Excel de ejemplo y **Exportar** descarga el inventario actual.

## Dónde se guardan los datos

El inventario importado, los choferes y el tema elegido se guardan en el navegador (`localStorage`) de la computadora donde se usa. No se comparten entre equipos: para eso hace falta un servidor o base de datos.

## Cómo usarla

Abrí `index.html` en el navegador (doble clic). Necesita internet para cargar la librería 3D (Three.js) y el lector de Excel (SheetJS) desde CDN.

Atajos: `/` enfoca el buscador · `Enter` abre el primer resultado · `Esc` limpia.

## Datos

Por ahora la flota y los pedidos son simulados; el inventario de ejemplo se reemplaza al importar un Excel. Para conectarla a datos reales hay que reemplazar los arreglos `TRUCKS_DEF`, `orders` y `PRODUCTS_DEF` por llamadas a la API/GPS/sistema de stock de la empresa.
