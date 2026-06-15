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

- **Tableros y carriles** configurables, con tarjetas que arrastrás entre estados y tableros.
- **Tarjetas completas**: prioridad, persona, vencimiento, etiquetas y checklist con responsables.
- **Mapa mental** por tarjeta: armás ideas, las anidás, colapsás ramas grandes y las pasás al checklist.
- **Pizarra** por tarjeta: cuadros, textos, imágenes y conexiones; exportás a PNG.
- **Vista Calendario** (mes/semana): ves las tarjetas por vencimiento y arrastrás para reprogramar.
- **Vista por persona** para ver la carga de cada uno y reasignar rápido.
- **Borrador rápido** para reuniones: lo usás y al final decidís si lo guardás o lo tirás.
- **Compartir** desde cada sección: resumen de la tarjeta, mapa o ítems como texto
  (copiar, WhatsApp o mail) y la pizarra como imagen.
- **Búsqueda y filtros**, archivar y tema claro/oscuro.
- **Backup**: exportás/importás todo en JSON.

> El detalle de cada versión y novedad está en el **[CHANGELOG](CHANGELOG.md)**.

## Cómo está hecho

Un único `index.html`: HTML + CSS + JavaScript **vanilla** (sin build, sin dependencias).
Todo el estado se guarda en `localStorage`. Si sabés JS, es fácil de leer y extender.

## Contribuir

¡Bienvenidas las contribuciones! Mirá [CONTRIBUTING.md](CONTRIBUTING.md).
`main` está protegida: los cambios entran por **Pull Request** y requieren aprobación.

## Licencia

[MIT](LICENSE).
