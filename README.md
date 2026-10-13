# Valsur Logística — Panel de control

Aplicación web de una sola página (`index.html`) con tema claro / oscuro (botón ☀/☾ arriba a la derecha).

**Versión publicada (para usar con los choferes):** https://claude.ai/artifact/VibC5LgNLhEa3ynt5cwoYb

Secciones:

- **Panel Jefe**: mapa 3D isométrico con la ubicación de todos los vehículos en tiempo real, estado de cada camión (cargando, en ruta, descargando, regresando, incidencia, en taller), pedidos a bordo con su línea de tiempo y tablero de pedidos por etapa (Recibido → En preparación → Cargado → En tránsito → Entregado).
- **Depósito**: buscador de mercadería por nombre, código, lote o ubicación (ej. `B-04`). Al elegir un producto se resalta su posición en la estantería 3D (pasillo, posición y nivel) y se muestra el detalle de lotes y vencimientos.

- **Pedidos**: la oficina carga los pedidos que entran (cliente, dirección, contacto, fecha, horario, bultos, peso, prioridad e indicaciones), los envía a un chofer y sigue cada uno: Sin asignar → Enviado → Aceptado → En camino → Entregado. Si el chofer rechaza o reporta un problema, aparece marcado y con aviso. Cada pedido tiene su propio chat oficina ↔ chofer, en reemplazo de WhatsApp. Se puede pegar el mensaje original del cliente y, en la versión publicada, completar el formulario con IA.
- **App chofer**: la pantalla que usa cada chofer en el celular: pedidos nuevos (Aceptar / Rechazar), en curso (Salir hacia el cliente, Entregado con nombre de quien recibió, Reportar problema) y entregados hoy, con botones para abrir Google Maps, llamar y escribir a la oficina. La oficina la ve como vista previa eligiendo un chofer.
- **Choferes**: perfiles con nombre, DNI, teléfono, categoría de licencia, vehículo asignado, vencimientos (licencia de conducir, LiNTI, psicofísico, cargas peligrosas, ART, libreta sanitaria) y habilitaciones. Se pueden agregar, editar y eliminar. Los vencidos o próximos a vencer (30 días) se marcan en rojo / amarillo, y el Panel Jefe avisa si el chofer de un camión tiene documentación vencida.

## Cómo se conectan oficina y choferes

La app publicada en Claude guarda pedidos, choferes, accesos e inventario en una base de datos compartida, y todos ven los cambios al instante.

1. Desde el botón **Compartir** del link, invitá a cada chofer y a cada persona de la oficina por email como **Editor**. Es el nivel que permite guardar cambios; cada uno necesita una cuenta de Claude.
2. Al entrar, cada persona elige si es chofer (y cuál es su perfil) o de oficina.
3. En **Choferes → Accesos a la app** aprobás o rechazás cada solicitud. Un chofer aprobado solo ve su app con sus pedidos.

Los avisos de pedido nuevo (sonido, vibración y contador en la pestaña) llegan mientras la app está abierta. No hay notificaciones push con el celular bloqueado.

Abierta como archivo local (`index.html`), la app funciona en modo demo: todo queda en ese navegador. Para probar el circuito, abrí dos pestañas, una en **Pedidos** y otra en **App chofer**.

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

En la versión publicada, pedidos, choferes, accesos e inventario se guardan en la base de datos compartida del link. En el archivo local, todo se guarda en el navegador (`localStorage`). El tema elegido siempre se guarda en cada navegador.

## Cómo usarla

Abrí `index.html` en el navegador (doble clic). Necesita internet para cargar la librería 3D (Three.js) y el lector de Excel (SheetJS) desde CDN.

Atajos: `/` enfoca el buscador · `Enter` abre el primer resultado · `Esc` limpia.

## Datos

El movimiento de la flota en el mapa sigue siendo simulado hasta conectar un GPS. Los pedidos y choferes son reales en la versión publicada. Para conectarla a datos reales hay que reemplazar los arreglos `TRUCKS_DEF`, `orders` y `PRODUCTS_DEF` por llamadas a la API/GPS/sistema de stock de la empresa.
