## ADDED Requirements

### Requirement: Acceso a la vista Calendario

El sistema SHALL ofrecer una vista Calendario como tercera opción del selector de vistas a
nivel tablero, junto a Tarjetas e Ítems. El sistema SHALL recordar la última vista elegida
entre sesiones usando `localStorage`.

#### Scenario: Cambiar a la vista Calendario
- **WHEN** el usuario hace clic en el botón "Calendario" del selector de vistas
- **THEN** se oculta el tablero y la vista de ítems, y se muestra la grilla del calendario
- **AND** el botón "Calendario" queda marcado como activo

#### Scenario: La vista elegida persiste
- **WHEN** el usuario está en la vista Calendario y recarga la página
- **THEN** la aplicación vuelve a abrir en la vista Calendario

### Requirement: Modos Mes y Semana

La vista Calendario SHALL soportar dos modos de despliegue, Mes y Semana, alternables por el
usuario. El sistema SHALL recordar el modo elegido entre sesiones.

#### Scenario: Modo Mes
- **WHEN** el usuario selecciona el modo "Mes"
- **THEN** la grilla muestra las semanas del mes ancla con sus 7 días (lunes a domingo)
- **AND** el encabezado indica el mes y el año

#### Scenario: Modo Semana
- **WHEN** el usuario selecciona el modo "Semana"
- **THEN** la grilla muestra los 7 días de la semana ancla
- **AND** el encabezado indica el rango de fechas de esa semana

### Requirement: Ubicación de tarjetas por vencimiento

El sistema SHALL ubicar cada tarjeta visible en la celda del día que coincide con su
vencimiento (`due`, formato `YYYY-MM-DD`). El sistema SHALL mostrar el día actual destacado.

#### Scenario: Tarjeta con vencimiento dentro del rango
- **WHEN** una tarjeta tiene `due` igual a un día presente en la grilla
- **THEN** la tarjeta aparece en la celda de ese día mostrando su título

#### Scenario: Día con varias tarjetas
- **WHEN** varias tarjetas vencen el mismo día
- **THEN** todas aparecen apiladas en la celda de ese día

#### Scenario: Día de hoy destacado
- **WHEN** se renderiza el calendario
- **THEN** la celda correspondiente a la fecha actual se muestra visualmente diferenciada

### Requirement: Tarjetas sin fecha

El sistema SHALL listar aparte, en una zona "Sin fecha", las tarjetas visibles que no tienen
vencimiento, para que no se pierdan y puedan agendarse.

#### Scenario: Tarjeta sin vencimiento
- **WHEN** una tarjeta visible tiene `due` vacío
- **THEN** no aparece en ninguna celda de día
- **AND** aparece en la zona "Sin fecha"

### Requirement: Reprogramar arrastrando

El usuario SHALL poder arrastrar una tarjeta a la celda de otro día para cambiar su
vencimiento. El cambio SHALL persistirse en `localStorage` y reflejarse de inmediato.

#### Scenario: Mover una tarjeta a otro día
- **WHEN** el usuario arrastra una tarjeta desde un día y la suelta en la celda de otro día
- **THEN** el `due` de la tarjeta pasa a ser la fecha de la celda destino
- **AND** la tarjeta se muestra en el día destino tras soltarla
- **AND** el cambio queda guardado al recargar

#### Scenario: Agendar una tarjeta sin fecha
- **WHEN** el usuario arrastra una tarjeta desde la zona "Sin fecha" y la suelta en un día
- **THEN** el `due` de la tarjeta pasa a ser la fecha de ese día
- **AND** la tarjeta deja de aparecer en la zona "Sin fecha"

### Requirement: Respeto de alcance y filtros

La vista Calendario SHALL mostrar el mismo conjunto de tarjetas que las otras vistas según
el tablero/foco actual y los filtros activos (búsqueda, prioridad, persona, archivadas,
búsqueda global).

#### Scenario: Filtro de persona activo
- **WHEN** hay un filtro de persona activo y el usuario abre el calendario
- **THEN** solo se muestran (en días y en "Sin fecha") las tarjetas que pasan ese filtro

#### Scenario: Tablero actual
- **WHEN** el usuario está en un tablero específico (no "Todos")
- **THEN** el calendario solo muestra tarjetas de ese tablero, con la misma regla que el resto

### Requirement: Navegación temporal

El usuario SHALL poder avanzar y retroceder el período mostrado y volver al período actual.

#### Scenario: Período anterior y siguiente
- **WHEN** el usuario usa los controles de anterior/siguiente
- **THEN** la grilla se desplaza un mes (modo Mes) o una semana (modo Semana) según el modo

#### Scenario: Volver a hoy
- **WHEN** el usuario usa el control "Hoy"
- **THEN** la grilla vuelve a mostrar el período que contiene la fecha actual

### Requirement: Apertura de tarjeta desde el calendario

El usuario SHALL poder abrir el editor de una tarjeta desde el calendario.

#### Scenario: Clic en una tarjeta del calendario
- **WHEN** el usuario hace clic (sin arrastrar) sobre una tarjeta del calendario
- **THEN** se abre el editor de esa tarjeta, igual que desde el tablero
