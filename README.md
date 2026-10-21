# Panel Comercial de Ventas

Dashboard estático para cargar un Excel de compras y explorar clientes, rutas y vendedores **sin mapa ni coordenadas**.

## Ejecutar en VS Code

1. Abrí esta carpeta en Visual Studio Code.
2. Instalá la extensión **Live Server** (Ritwick Dey).
3. Hacé clic derecho en `index.html` y seleccioná **Open with Live Server**.
4. Presioná **Cargar Excel** y seleccioná el archivo de ventas.

Necesitás conexión a Internet para descargar Chart.js y SheetJS desde sus CDN. El Excel se lee y procesa dentro del navegador: no se envía a esos CDN.

## Funciones

- Indicadores: facturación, clientes activos, transacciones, compra media por cliente.
- Gráficos: tendencia mensual, segmentos de clientes, vendedores, rutas, ranking de clientes.
- Filtros combinados: fechas, vendedor, ruta, segmento por importe acumulado, nombre/código de cliente.
- Tabla paginada y ordenable por nombre, transacciones y importe.
- Exportación CSV de clientes tras aplicar filtros.
- Subtotales y total general del Excel no se contabilizan como compras; los códigos con ceros iniciales se conservan.
- Cambiar de archivo reemplaza el anterior.

**Criterio de filtrado:** fecha/vendedor/ruta se aplican primero a las transacciones; luego se recalculan compras y el segmento comercial de cada cliente para el período seleccionado. El filtro de segmento se aplica sobre esos nuevos totales. Un mismo cliente puede aparecer con “Varios vendedores” o “Varias rutas” si sus compras filtradas pertenecen a varios.

## GitHub Pages

Subí el contenido de esta carpeta a un repositorio y entrá a **Settings → Pages → Deploy from a branch → main / (root)**. El sitio es público; no publiques el Excel ni un export CSV. El repositorio incluye solo código y estilos, sin ventas reales.

## Excel requerido

Debe contener la hoja `ORIGINAL` con las columnas: `Fecha`, `Cliente`, `Nombre`, `MONTO`, `TOTAL CLIENTE`, `Ruta`, `Vendedor`, `DIRECCION`. Se toma `MONTO` de cada venta y se excluyen las filas `TOTAL CLIENTE`.

## Límites

- En GitHub Pages no hay autenticación real ni almacenamiento servidor.
- Las funciones de gráficos requieren Chart.js; la lectura XLSX requiere SheetJS.
- Los filtros de rango usan fecha de transacción y los montos son en colones costarricenses.
- Los posibles duplicados idénticos no se eliminan automáticamente para no borrar compras legítimas.

## Versión geográfica (actualización)

**Usá el Excel `Ventas_Con_Codigos_Postales.xlsx`**. La aplicación reconoce automáticamente la hoja `VENTAS_GEO` y también sigue admitiendo la hoja histórica `ORIGINAL` (en ese caso, no hay mapa con datos). El archivo no debe subirse a GitHub: cargalo desde la computadora.

- Se agregaron filtros de **provincia** y **estado de ubicación**; afectan tanto los gráficos como el mapa.
- El mapa muestra un **coroplético de distrito**, NO marcadores de farmacias. Al seleccionar un distrito, muestra los clientes y montos acumulados del período filtrado.
- Por defecto se incluyen solamente las ubicaciones marcadas `CONFIRMADO` en el Excel. Podés habilitar ubicaciones `PROBABLE` con la casilla del mapa. **Estos estados proceden del Excel previo y no garantizan verificación postal independiente**.
- Las asignaciones cuyo código postal no coincida con un polígono se excluyen del mapa, no se reubican arbitrariamente.
- Los polígonos se descargan en tiempo real desde el servicio de distritos de **ARESEP**, con créditos a su cartografía. La capa externa puede cambiar o fallar; si eso pasa, el mapa muestra un aviso y el resto del dashboard continúa funcionando.
- Capas base de OpenStreetMap; JavaScript de Leaflet vía CDN. Se requiere conexión a Internet para mapas, SheetJS y Chart.js. La aplicación no envía transacciones ni montos a los proveedores de mapas.
- Para mapas comparativos por distrito, revisá primero la exactitud de los códigos postales: el archivo unificado conserva códigos provisionales y registros pendientes.
