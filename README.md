# mindboard

**Tablero kanban en un solo archivo HTML.** Sin servidor, sin frameworks, sin
dependencias, sin internet. Lo abrís con doble clic y funciona offline. Pensado para
reemplazar las anotaciones en papel y para pensar en vivo durante reuniones.

> Tus datos viven en el **localStorage de tu navegador** — nunca salen de tu máquina y el
> archivo HTML que compartís va siempre vacío.

## Probarlo

1. Descargá [`index.html`](index.html).
2. Doble clic → abre en tu navegador (`file://`).

O serví la carpeta con cualquier server estático / GitHub Pages.

## Qué incluye

- **Tableros (temas)** en un menú lateral colapsable (a barra de iconos con tooltips),
  reordenables con drag & drop. Vista **★ Hoy** y **Todos**.
- **Carriles (estados)** configurables (crear, renombrar, recolorear, reordenar
  arrastrando el encabezado). Alta de tarjetas inline al pie de cada carril.
- **Tarjetas** con título, descripción, URL, prioridad, persona, vencimiento, etiquetas
  y checklist. Drag & drop entre carriles y tableros.
- **Checklist con responsables**: cada ítem con persona, prioridad y vencimiento.
- **Secciones por tarjeta**: Editar · **Mapa mental** · **Pizarra** · Ítems.
  - *Mapa mental*: árbol de ideas, drag & drop para reordenar/mover ramas, Enter crea la
    siguiente idea, Tab crea una sub-idea, y podés mandar nodos al checklist.
  - *Pizarra*: cuadros, textos e imágenes; conectás nodos arrastrando desde sus puntos;
    duplicar/copiar/pegar; doble clic crea; exportás a PNG.
  - Exportás el mapa y los ítems como **texto** para copiar/compartir.
- **Vista Tarjetas / Ítems**: ítems y tarjetas **agrupados por persona** para ver la carga
  de cada uno y reasignar rápido.
- **Borrador rápido**: una tarjeta descartable para usar en una reunión; al cerrarla
  decidís si la guardás (y dónde) o la descartás.
- **Personas** administradas desde el menú, marcás cuál sos "vos" (aparecés primero al
  asignar).
- **Barra de tarjetas recientes** para saltar entre las que estás trabajando.
- Búsqueda + filtros, archivar, copiar/pegar listas, **exportar/importar** backup JSON,
  recordatorio de backup, tema claro/oscuro.

## Cómo está hecho

Un único `index.html`: HTML + CSS + JavaScript **vanilla** (sin build, sin dependencias).
Todo el estado se guarda en `localStorage`. Si sabés JS, es fácil de leer y extender.

## Contribuir

¡Bienvenidas las contribuciones! Mirá [CONTRIBUTING.md](CONTRIBUTING.md).
`main` está protegida: los cambios entran por **Pull Request** y requieren aprobación.

## Licencia

[MIT](LICENSE).
