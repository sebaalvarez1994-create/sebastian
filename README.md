# Valsur Logística — Panel de control

Aplicación web de una sola página (`index.html`) con tema claro / oscuro (botón ☀/☾ arriba a la derecha).

**Versión publicada (para usar con los choferes):** https://claude.ai/artifact/VibC5LgNLhEa3ynt5cwoYb

Secciones:

- **Panel Jefe**: mapa 3D isométrico con la ubicación de todos los vehículos en tiempo real, estado de cada camión (cargando, en ruta, descargando, regresando, incidencia, en taller), pedidos a bordo con su línea de tiempo y tablero de pedidos por etapa (Recibido → En preparación → Cargado → En tránsito → Entregado).
- **Depósito**: buscador de mercadería por nombre, código, lote o ubicación (ej. `B-04`). Al elegir un producto se resalta su posición en la estantería 3D (pasillo, posición y nivel) y se muestra el detalle de lotes y vencimientos.

- **Pedidos**: la oficina carga los pedidos que entran (cliente, dirección, contacto, fecha, horario, bultos, peso, prioridad e indicaciones), los envía a un chofer y sigue cada uno: Sin asignar → Enviado → Aceptado → En camino → Entregado. Si el chofer rechaza o reporta un problema, aparece marcado y con aviso. Cada pedido tiene su propio chat oficina ↔ chofer, en reemplazo de WhatsApp. Se puede pegar el mensaje original del cliente y, en la versión publicada, completar el formulario con IA.
- **App chofer**: la pantalla que usa cada chofer en el celular: pedidos nuevos (Aceptar / Rechazar), en curso (Salir hacia el cliente, Entregado con nombre de quien recibió, Reportar problema) y entregados hoy, con botones para abrir Google Maps, llamar y escribir a la oficina. La oficina la ve como vista previa eligiendo un chofer.
- **Choferes**: perfiles con nombre, DNI, teléfono, categoría de licencia, vehículo asignado, vencimientos (licencia de conducir, LiNTI, psicofísico, cargas peligrosas, ART, libreta sanitaria) y habilitaciones. Se pueden agregar, editar y eliminar. Los vencidos o próximos a vencer (30 días) se marcan en rojo / amarillo, y el Panel Jefe avisa si el chofer de un camión tiene documentación vencida.

## Usuarios

Al abrir la app se pide usuario y contraseña. Cada sector ve solo lo suyo:

| Usuario | Contraseña | Qué ve |
|---|---|---|
| `jefe` | `jefe` | Toda la app: mapa, pedidos, depósito, choferes y la vista previa de la app chofer. Es el único que importa/exporta Excel, edita choferes y manda camiones al taller. |
| `pedidos` | `pedidos` | Pedidos y los perfiles de los choferes (sin poder editarlos). |
| `choferes` | `choferes` | Solo la App chofer. La primera vez cada chofer elige su nombre y el celular lo recuerda. |
| `deposito` | `deposito` | Solo el depósito: mapa, buscador y despacho de mercadería. |

La sesión queda abierta en ese navegador hasta tocar **Salir**. Las contraseñas no están escritas en el código (se compara una huella), pero esto es un control de acceso simple entre sectores, no una protección fuerte: quien tenga el link y conocimientos técnicos podría saltearlo.

## El mapa de la flota

El Panel Jefe muestra un mapa simbólico: la base Valsur, el taller mecánico, cuatro rutas de reparto y una calle de **demorados** con conos. Cada camión se ubica solo, según los pedidos de su chofer:

- **En base**: el chofer no tiene pedidos activos.
- **Preparando** (al inicio de una ruta): tiene un pedido enviado o aceptado.
- **En ruta** (avanza por la ruta): salió hacia el cliente; en el final de la ruta se ve el nombre del cliente.
- **Demorado** (calle con conos): el chofer reportó un problema.
- **En taller**: el jefe lo mandó al taller desde el panel del vehículo.

### Editar la flota y seguimiento GPS

El usuario **jefe** edita la flota desde **Panel general → Flota → Editar flota** (o desde el panel de cada vehículo): agrega, modifica o quita vehículos (patente, modelo, tipo, año, capacidad, notas). Si cambia una patente, el chofer asignado sigue vinculado; si quita un vehículo, su chofer queda sin vehículo asignado.

Arriba del mapa hay dos pestañas: **Mapa ilustrativo** (el mapa 3D) y **Mapa real (GPS)**. La página publicada en Claude no puede mostrar mapas de calles ni conectarse al servidor del GPS, así que el mapa real lista cada vehículo con su estado, chofer y destino, y un botón **Ver en vivo** que abre el seguimiento en la plataforma del proveedor de GPS. Para activarlo, en **Editar flota** se pega el link de seguimiento de cada vehículo (el link para compartir ubicación que da el proveedor) y, opcionalmente, el nombre del proveedor y el link de su plataforma.

## Despachos del depósito

En **Depósito**, al elegir un producto aparece **Despachar**: se elige la ubicación, la cantidad y, opcionalmente, el pedido al que va. El stock se descuenta al instante (si la posición queda vacía, se libera) y el botón **Despachos** muestra el registro de salidas con fecha, cantidad, destino y quién lo hizo.

## Cómo se conectan oficina y choferes

La app publicada en Claude guarda pedidos, choferes, vehículos en taller, despachos e inventario en una base de datos compartida, y todos ven los cambios al instante. Cada persona que la use tiene que estar invitada al link como **Editor** (botón Compartir) para poder guardar cambios; después entra con el usuario de su sector.

Los avisos de pedido nuevo (sonido, vibración y contador en la pestaña) llegan mientras la app está abierta. No hay notificaciones push con el celular bloqueado.

Abierta como archivo local (`index.html`), la app funciona en modo demo: todo queda en ese navegador.

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

En la versión publicada, pedidos, choferes, vehículos, despachos e inventario se guardan en la base de datos compartida del link. En el archivo local, todo se guarda en el navegador (`localStorage`). El tema elegido siempre se guarda en cada navegador.

## Cómo usarla

Abrí `index.html` en el navegador (doble clic). Necesita internet para cargar la librería 3D (Three.js) y el lector de Excel (SheetJS) desde CDN.

Atajos: `/` enfoca el buscador · `Enter` abre el primer resultado · `Esc` limpia.

## Datos

El mapa no usa GPS: ubica cada camión según el estado de los pedidos de su chofer. Para conectarla a datos reales hay que reemplazar los arreglos `TRUCKS_DEF`, `orders` y `PRODUCTS_DEF` por llamadas a la API/GPS/sistema de stock de la empresa.
