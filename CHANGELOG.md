# Changelog

Todos los cambios relevantes de **mindboard** se anotan acá.

El formato sigue (libremente) [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/)
y el versionado es [SemVer](https://semver.org/lang/es/). Las entradas nuevas se agregan
arriba, en **No publicado**, y pasan a una versión con fecha cuando se publican.

## [No publicado]

### Corregido

- **Mapa mental — scroll fantasma.** El conector vertical del *primer hijo* de cada rama se
  extendía un alto completo de su `<li>` (que incluye todo su subárbol), desbordando hacia
  abajo. En ramas con un primer hijo de subárbol grande esto generaba cientos de píxeles de
  espacio vacío scrolleable "hacia la nada" y hacía que los nodos parecieran quedar flotando
  lejos de su padre. Ahora la línea del primer hijo va del centro al borde inferior (`height:50%`).

### Agregado

- **Planilla — valores porcentuales.** Ahora podés escribir `70%` (o `12,5%`) y la celda lo
  muestra como porcentaje pero lo usa como `0.7` en los cálculos (`=A1*B1`, `=SUMA(...)`, etc.).
  El formato `%` se conserva al exportar a Excel.

- **Mapa mental — colapsar / expandir todo.** Dos botones en el modal del mapa para colapsar
  todas las ramas de una vez (quedan visibles solo los nodos de primer nivel, con el conteo
  de descendientes ocultos) o expandirlas todas. El estado se guarda como hasta ahora.

- **Mapa mental — descargar como imagen (.png).** Nuevo botón en el modal del mapa que
  exporta lo que está renderizado (respeta ramas colapsadas) a una imagen. Se arma un SVG a
  partir de la geometría en vivo (cajas, texto, acentos por nivel y conectores) y se rasteriza
  sin librerías; queda nítido en pantallas retina.

- **Secciones a pantalla completa.** Botón de pantalla completa en la barra de secciones de
  la tarjeta (Editar, Mapa, Pizarra, Planilla, Ítems): el modal pasa a ocupar todo el
  viewport y se oculta lo accesorio (barra de recientes, ayudas) para **maximizar el área de
  trabajo**. Se mantiene al cambiar de sección y se sale al volver al tablero (o con el mismo
  botón).

- **Planilla de cálculo en la tarjeta.** Nueva sección de la tarjeta (junto a Editar, Mapa,
  Pizarra e Ítems) para tener a mano un bloc de cálculos sin salir del tablero:
  - Grilla simple que arranca en 6×12 y crece con **+ Fila** / **+ Columna** (hasta 26×200).
  - **Fórmulas básicas**: aritmética entre celdas (`=A1+B2*3`) y funciones sobre rangos
    `SUMA`/`PROMEDIO`/`MIN`/`MAX`/`CONTAR` (`=SUMA(A1:A10)`); detecta ciclos y división por cero.
  - **Formato**: negrita, alineación y pintado de celdas con colores semánticos (respeta dark mode).
  - **Barra de totales** (Suma · Promedio · Cuenta) sobre la selección, y selección por arrastre.
  - **Pegar desde Excel**: pegás un rango tabulado y se arma sola; **copiar** devuelve TSV.
  - **Copiar/pegar adapta las fórmulas**: al copiar una celda (o rango) y pegarla en otra
    posición de la misma planilla, las referencias relativas se ajustan a la fila/columna
    nueva (copiar `=A1+B1` y pegar una fila abajo ⇒ `=A2+B2`), como en Excel/Sheets. El
    pegado desde afuera sigue siendo literal.
  - **Descargar como `.xlsx`** (Excel de verdad, armado a mano sin librerías): conserva valores,
    fórmulas (traducidas a `SUM`/`AVERAGE`/`COUNT`) y formato, para continuarlo o compartirlo.
  - La tarjeta muestra un **badge** con la cantidad de celdas con datos.
  - **Fórmulas estilo planilla ("point mode")**: al editar una fórmula, moverte con las flechas
    o clickear/arrastrar celdas inserta su referencia (`A1`, rango `A1:B3`); `+` al inicio
    arranca una fórmula (`+A1` ⇒ `=A1`). Navegación al confirmar: `Enter` baja, `Tab` derecha,
    **`Shift+Tab` izquierda**, `Escape` descarta.

- **Mapa mental — colapsar ramas.** Cada nodo con hijos muestra un caret para colapsar/
  expandir su rama; colapsado indica cuántos descendientes oculta (**▸ N**). El estado se
  recuerda y agregar un sub-ítem expande la rama automáticamente. Útil para mapas grandes.

### Cambiado

- **Más color en los componentes nuevos** (con tokens semánticos, respeta el dark mode):
  nodos del mapa coloreados por nivel de rama y conectores con un toque de marca; tarjetas
  del calendario tintadas por prioridad y día de hoy resaltado.

## [1.1.0] - 2026-06-14

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

[No publicado]: https://github.com/rama77/mindboard/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/rama77/mindboard/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/rama77/mindboard/releases/tag/v1.0.0
