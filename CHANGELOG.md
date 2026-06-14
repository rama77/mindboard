# Changelog

Todos los cambios relevantes de **mindboard** se anotan acá.

El formato sigue (libremente) [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/)
y el versionado es [SemVer](https://semver.org/lang/es/). Las entradas nuevas se agregan
arriba, en **No publicado**, y pasan a una versión con fecha cuando se publican.

## [No publicado]

### Agregado

- **Vista Calendario.** Tercera vista del tablero (junto a Tarjetas e Ítems) que ubica las
  tarjetas por su fecha de vencimiento:
  - Modos **Mes** y **Semana** (lunes a domingo), con navegación anterior/siguiente y **Hoy**.
  - **Arrastrar** una tarjeta a otro día cambia su vencimiento; las tarjetas **sin fecha** se
    listan aparte y se agendan arrastrándolas a un día. Clic en una tarjeta abre su editor.
  - Respeta el tablero/foco y los filtros activos; recuerda vista, modo y período.
  - Primer cambio hecho con **OpenSpec** (propuesta + spec + diseño + tareas en `openspec/`).
- **Compartir desde cada sección de la tarjeta.** Botón de compartir (mismo ícono y lugar)
  en Editar, Mapa mental, Ítems y Pizarra:
  - *Editar*: genera un **resumen de la tarjeta** en texto (título, tablero, estado,
    prioridad, responsable, vencimiento, etiquetas, descripción y checklist) a partir de
    lo que tengas en el formulario, incluso sin guardar.
  - *Mapa mental* e *Ítems*: comparten su contenido como texto.
  - El modal de compartir ahora ofrece **Copiar**, **WhatsApp** (abre `wa.me` con el texto)
    y **Mail** (abre el cliente con `mailto:`, asunto = primera línea).
  - *Pizarra*: comparte descargando la imagen `.png` para adjuntar.

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
