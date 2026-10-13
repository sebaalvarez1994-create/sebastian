# Valsur Logística — Panel de control

Aplicación web de una sola página (`index.html`) con dos secciones:

- **Panel Jefe**: mapa 3D isométrico con la ubicación de todos los vehículos en tiempo real, estado de cada camión (cargando, en ruta, descargando, regresando, incidencia, en taller), pedidos a bordo con su línea de tiempo y tablero de pedidos por etapa (Recibido → En preparación → Cargado → En tránsito → Entregado).
- **Depósito**: buscador de mercadería por nombre, código, lote o ubicación (ej. `B-04`). Al elegir un producto se resalta su posición en la estantería 3D (pasillo, posición y nivel) y se muestra el detalle de lotes y vencimientos.

## Cómo usarla

Abrí `index.html` en el navegador (doble clic). Necesita internet para cargar la librería 3D (Three.js) desde CDN.

Atajos: `/` enfoca el buscador · `Enter` abre el primer resultado · `Esc` limpia.

## Datos

Por ahora los datos son simulados (flota, pedidos e inventario se generan en el propio archivo). Para conectarla a datos reales hay que reemplazar los arreglos `TRUCKS_DEF`, `orders` y `PRODUCTS_DEF` por llamadas a la API/GPS/sistema de stock de la empresa.
