## Why

Hoy las tarjetas tienen vencimiento (`due`), pero la única forma de ver "qué cae cuándo"
es recorrer los carriles tarjeta por tarjeta. Falta una mirada temporal: ver de un vistazo
qué hay esta semana / este mes y reprogramar rápido. Una **Vista Calendario** cubre ese
hueco reutilizando datos que ya existen, sin agregar dependencias.

## What Changes

- Se agrega una tercera vista a nivel tablero, junto a **Tarjetas** e **Ítems**: el toggle
  pasa a tener **Tarjetas · Ítems · Calendario**.
- La vista Calendario ubica cada tarjeta en su **fecha de vencimiento** (`due`), con dos
  modos: **Mes** y **Semana**.
- Se puede **arrastrar una tarjeta a otro día** para cambiarle el vencimiento (persistido en
  `localStorage`, igual que el drag entre carriles ya cambia el estado).
- Las tarjetas **sin vencimiento** no ocupan días; se listan aparte en una zona **"Sin
  fecha"** desde donde también se las puede arrastrar a un día para asignarles vencimiento.
- La vista respeta el **tablero/foco actual y los filtros** (búsqueda, prioridad, persona)
  con la misma lógica que las otras vistas (`taskInView` + `matches`).
- Clic en una tarjeta del calendario abre su **editor**, igual que en el tablero.
- Navegación temporal: ir al **mes/semana anterior y siguiente**, y un botón **Hoy**.

No hay cambios que rompan datos: `due` ya existe y mantiene su formato `YYYY-MM-DD`.

## Capabilities

### New Capabilities
- `vista-calendario`: vista temporal del tablero (modos mes y semana) que muestra las
  tarjetas por fecha de vencimiento, permite reprogramar arrastrando, contempla las
  tarjetas sin fecha y respeta tablero/foco y filtros activos.

### Modified Capabilities
<!-- No hay specs previas en openspec/specs/. Las vistas actuales (Tarjetas/Ítems) no tienen
     spec escrita, así que no se modifica ninguna capability existente: solo se agrega una. -->

## Impact

- **Código**: `index.html` únicamente.
  - HTML: un botón más en el toggle de vistas (`#btnViewCal`) y un contenedor `#calView`.
  - CSS: estilos de la grilla del calendario usando **tokens semánticos** existentes
    (`--card`, `--border`, `--muted`, `--warning`, `--danger`, etc.).
  - JS: `viewMode` admite `'cal'`; `render()` muestra/oculta `#calView`; nuevas funciones
    `renderCalendar()` + helpers de fechas (sin librerías). El drop de tarjetas en un día
    setea `t.due` y guarda.
- **Datos**: se reutiliza `t.due`. Se persiste el modo elegido (mes/semana) y, opcionalmente,
  la fecha "ancla" de navegación bajo una clave `localStorage` nueva (`mindboard*.v1`).
- **Sin** dependencias nuevas, sin red, sin build. Offline-first intacto.
