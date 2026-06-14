## Context

mindboard es un único `index.html` (HTML + CSS + JS vanilla, sin build ni dependencias),
con datos en `localStorage`. Hoy hay dos vistas a nivel tablero conmutadas por la variable
`viewMode` (`'cards' | 'items'`): `render()` muestra/oculta `#board` o `#itemsView` con
`style.display`. Las tarjetas ya tienen `due` (string `YYYY-MM-DD` o `""`), y existe
`dueInfo(due)` que parsea con `new Date(due+"T00:00:00")`. El alcance/filtros se resuelven
con `taskInView(t)` + `matches(t, currentFilters())`. El drag entre carriles ya persiste
cambios de estado en `localStorage`. Esta vista debe encajar en ese patrón, sin librerías.

## Goals / Non-Goals

**Goals:**
- Tercera vista (`viewMode === 'cal'`) con modos Mes y Semana.
- Ubicar tarjetas por `due`; día de hoy destacado; zona "Sin fecha".
- Reprogramar arrastrando una tarjeta a otro día (y agendar desde "Sin fecha").
- Respetar tablero/foco y filtros con la MISMA lógica que las otras vistas.
- Abrir el editor al hacer clic en una tarjeta.
- Persistir vista activa, modo (mes/semana) y fecha ancla.
- Cero dependencias, offline, tokens de color semánticos.

**Non-Goals:**
- Eventos con hora del día / franjas horarias (solo día completo, como `due`).
- Vista de día único, agenda anual, o multi-día (rango con inicio y fin).
- Recurrencia, recordatorios o notificaciones.
- Reordenar tarjetas dentro de un mismo día (el orden dentro del día no es significativo).

## Decisions

- **Fechas sin librería, en horario local.** Se parsea/compone con `new Date(y, m, d)` y se
  formatea la clave del día como `YYYY-MM-DD` con un helper `ymd(date)` (pad manual). Se
  comparan días por su string `YYYY-MM-DD`, evitando problemas de huso/UTC (mismo criterio
  que `dueInfo`, que ya ancla a medianoche local). Alternativa descartada: comparar objetos
  `Date` con horas → frágil por husos y DST.
- **Semana de lunes a domingo.** Coherente con uso laboral local. El inicio de semana se
  calcula desde el ancla. Alternativa (domingo-sábado) descartada por contexto de trabajo.
- **Estado nuevo mínimo.** Se agrega `viewMode='cal'` y dos variables: `calMode`
  (`'month'|'week'`) y `calAnchor` (un `YYYY-MM-DD` dentro del período mostrado). Se
  persisten en `localStorage` bajo claves nuevas `mindboard*.v1` (p. ej. `mindboardVista.v1`
  para la vista activa y `mindboardCal.v1` para `{mode, anchor}`), siguiendo el patrón de
  claves existente. La vista activa se persiste para cumplir el requisito de recordar la
  última vista.
- **Reutilización total de alcance/filtros.** El conjunto base es
  `tasks.filter(t => taskInView(t) && matches(t, currentFilters()))`, idéntico a las otras
  vistas: así cualquier filtro/búsqueda/tablero aplica sin duplicar reglas.
- **Drag & drop aislado por tipo.** El tablero ya usa varios "tipos" de drag. El calendario
  usa su propio identificador en memoria (p. ej. `calDragId`) y sus celdas son las únicas
  drop targets de esta vista; al soltar, setea `t.due = fechaCelda`, `saveTasks()` y
  `renderCalendar()`. No comparte handlers con el drag de carriles/mapa/pizarra para no
  romperlos (gotcha conocido del proyecto).
- **Clic vs arrastre.** Para abrir el editor solo en clic real, se marca un flag al
  `dragstart` y, en `click`, se ignora si hubo arrastre (mismo criterio que el resto del
  código que distingue mover de abrir). Alternativa (abrir siempre en click) descartada
  porque dispararía al terminar de arrastrar.
- **Render por reemplazo de contenedor.** Igual que `renderItemsView()`, `renderCalendar()`
  reconstruye el HTML de `#calView` en cada cambio (datos chicos: tareas de un tablero).
  Simple y consistente; no hace falta diffing.
- **CSS con tokens.** Grilla de 7 columnas con `display:grid`; celdas con `--card`/`--border`,
  hoy con `--warning`/acento, vencidas usando los criterios ya existentes en `dueInfo`
  (`over`/`today`). Sin colores hardcodeados.

## Risks / Trade-offs

- **[Husos/medianoche]** comparaciones de fecha mal hechas muestran la tarjeta un día
  corrido → Mitigación: trabajar siempre con la clave `YYYY-MM-DD` local y nunca con
  `toISOString()` (que es UTC).
- **[Romper otros drag & drop]** el proyecto ya sufrió de eso → Mitigación: estado de drag
  propio del calendario y drop targets acotadas a celdas de `#calView`; no tocar handlers
  existentes.
- **[Días con muchas tarjetas]** una celda podría desbordar → Mitigación: la celda hace
  scroll vertical interno; opcionalmente mostrar un contador "+N" (decisión menor, no
  bloqueante para la spec).
- **[Click accidental tras drag]** abriría el editor sin querer → Mitigación: flag
  movido/arrastrado descrito arriba.
- **[Tamaño de `index.html`]** crece el archivo único → aceptado: es la regla de oro del
  proyecto; se mantiene el estilo y se reutiliza CSS/JS existente para minimizar el delta.

## Migration Plan

No hay migración de datos: `due` ya existe con el mismo formato. Si una versión vieja no
conoce las claves nuevas de `localStorage`, simplemente arranca en la vista por defecto
(`cards`) — degradación segura. Rollback = revertir el PR; los datos quedan intactos.

## Open Questions

- ¿Mostrar contador "+N" cuando un día tiene muchas tarjetas, o solo scroll interno? (No
  bloquea la spec; se puede decidir en implementación.)
- ¿La zona "Sin fecha" va fija al costado/arriba de la grilla o colapsable? (Detalle de UI.)
