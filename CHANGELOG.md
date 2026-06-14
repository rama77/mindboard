# Changelog

Todos los cambios relevantes de **mindboard** se anotan acá.

El formato sigue (libremente) [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/)
y el versionado es [SemVer](https://semver.org/lang/es/). Las entradas nuevas se agregan
arriba, en **No publicado**, y pasan a una versión con fecha cuando se publican.

## [No publicado]

_(Próximos cambios van acá.)_

## [1.0.0] - 2026-06-14

Primera versión pública. mindboard es un **tablero kanban en un solo `index.html`**:
HTML + CSS + JavaScript vanilla, sin build, sin dependencias, sin internet. Se abre con
doble clic y funciona offline; los datos viven en el `localStorage` del navegador.

### Agregado

- **Tableros (temas)** en un menú lateral colapsable (a barra de iconos con tooltips),
  reordenables con drag & drop. Vistas **★ Hoy** y **Todos**.
- **Carriles (estados)** configurables: crear, renombrar, recolorear y reordenar
  arrastrando el encabezado. Alta de tarjetas inline al pie de cada carril.
- **Tarjetas** con título, descripción, URL, prioridad, persona, vencimiento, etiquetas
  y checklist. Drag & drop entre carriles y entre tableros.
- **Checklist con responsables**: cada ítem con persona, prioridad y vencimiento.
- **Secciones por tarjeta** (control segmentado): Editar · Mapa mental · Pizarra · Ítems.
- **Mapa mental**: árbol de ideas; Enter crea la siguiente idea, Tab crea una sub-idea,
  Enter en vacío la borra; drag & drop para reordenar y mover ramas; podés mandar nodos
  al checklist; export a texto.
- **Pizarra**: cuadros, textos e imágenes; conexión de nodos arrastrando desde sus puntos;
  duplicar/copiar/pegar; doble clic crea; exportación a PNG.
- **Vista Tarjetas / Ítems**: agrupadas por persona para ver la carga de cada uno y
  reasignar rápido.
- **Borrador rápido**: una tarjeta descartable para reuniones; al cerrarla decidís si la
  guardás (y dónde) o la descartás.
- **Personas** administradas desde el menú; marcás cuál sos "vos" (aparecés primero al
  asignar).
- **Barra de tarjetas recientes** para saltar entre las que estás trabajando.
- **Búsqueda y filtros**, archivar, copiar/pegar listas.
- **Exportar / importar** backup en JSON, con recordatorio de backup.
- **Tema claro / oscuro**.
- **Mapa mental — pegar una lista**: al pegar texto multilínea en un nodo se crea una idea
  por línea, respetando la sangría para anidar ramas y sub-ramas (inverso del export) y
  limpiando bullets (`-`, `*`, `•`, `1.`). Después se reordena con drag & drop.

### Corregido

- **Mapa mental**: los nodos nuevos arrancan **sin texto** (antes aparecía "Nuevo" como
  placeholder); si confirmás un nodo vacío con Enter o al perder el foco, se descarta y no
  queda basura.

[No publicado]: https://github.com/rama77/mindboard/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/rama77/mindboard/releases/tag/v1.0.0
