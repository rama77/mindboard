## 1. Andamiaje de la vista

- [x] 1.1 Agregar el botón "Calendario" (`#btnViewCal`) al toggle de vistas, junto a Tarjetas e Ítems.
- [x] 1.2 Agregar el contenedor `#calView` (oculto por defecto) en el HTML, hermano de `#board` y `#itemsView`.
- [x] 1.3 Extender `viewMode` para aceptar `'cal'` y conmutar display de `#calView` en `render()` (ocultando los otros).
- [x] 1.4 Cablear el listener del botón para setear `viewMode='cal'`, marcar activo y llamar a `render()`.

## 2. Estado y persistencia

- [x] 2.1 Agregar variables `calMode` (`'month'|'week'`) y `calAnchor` (`YYYY-MM-DD`).
- [x] 2.2 Definir claves nuevas en `localStorage` (`mindboardVista.v1` para la vista activa, `mindboardCal.v1` para `{mode, anchor}`) y leerlas al iniciar con defaults seguros.
- [x] 2.3 Persistir vista activa al cambiar de vista, y `mode`/`anchor` al cambiarlos.

## 3. Helpers de fecha (sin librerías)

- [x] 3.1 `ymd(date)` → string `YYYY-MM-DD` en horario local (pad manual).
- [x] 3.2 `startOfWeek(date)` (lunes) y `addDays/addMonths` para navegación.
- [x] 3.3 `daysForView(anchor, mode)` → array de fechas a renderizar (semana = 7; mes = semanas completas lunes-domingo que cubren el mes).

## 4. Render del calendario

- [x] 4.1 `renderCalendar()` que arma el conjunto base `tasks.filter(t => taskInView(t) && matches(t, currentFilters()))`.
- [x] 4.2 Agrupar tarjetas por su `due` (clave `YYYY-MM-DD`); las de `due` vacío van al grupo "Sin fecha".
- [x] 4.3 Dibujar encabezado (mes/año o rango de semana) y controles: modo Mes/Semana, anterior, siguiente, Hoy.
- [x] 4.4 Dibujar la grilla de 7 columnas con los días del período; cada celda lista sus tarjetas (título + señal de prioridad/vencido reutilizando criterios de `dueInfo`).
- [x] 4.5 Destacar visualmente la celda del día actual.
- [x] 4.6 Dibujar la zona "Sin fecha" con sus tarjetas.

## 5. Interacción

- [x] 5.1 Drag de tarjetas dentro del calendario usando un estado propio (`calDragId`), sin tocar los otros drag & drop.
- [x] 5.2 Drop en una celda de día → setear `t.due = fechaCelda`, `saveTasks()`, `renderCalendar()`.
- [x] 5.3 Drop desde "Sin fecha" a un día → asignar `due` y quitarla de "Sin fecha".
- [x] 5.4 Click (sin arrastre) en una tarjeta → abrir el editor; usar flag de "arrastrado" para no abrir tras un drag.
- [x] 5.5 Botones anterior/siguiente avanzan mes o semana según `calMode`; "Hoy" lleva el ancla al período actual.

## 6. Estilos

- [x] 6.1 CSS de la grilla (`display:grid`, 7 columnas) y celdas usando tokens semánticos (`--card`, `--border`, `--muted`).
- [x] 6.2 Estilos de hoy (`--warning`/acento) y de tarjetas vencidas (`over`/`today`) sin colores hardcodeados.
- [x] 6.3 Scroll interno en celdas con muchas tarjetas; estilos de la zona "Sin fecha".
- [x] 6.4 Verificar que respeta el dark mode (tokens) y el estilo visual existente.

## 7. Verificación

- [x] 7.1 Probar en navegador todos los escenarios de la spec (ubicación por fecha, sin fecha, mes/semana, navegación, hoy, filtros, abrir editor).
- [x] 7.2 Verificar persistencia (vista, modo y ancla) recargando.
- [x] 7.3 Verificar que arrastrar entre carriles, mapa mental y pizarra siguen funcionando (no se rompió el D&D existente).
- [x] 7.4 Consola sin errores y chequeo de sintaxis JS (`node --check` sobre el `<script>` extraído).
- [x] 7.5 Actualizar CHANGELOG (No publicado) y README cuando la feature esté lista.
