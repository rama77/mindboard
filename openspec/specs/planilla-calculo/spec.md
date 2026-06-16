# planilla-calculo Specification

## Purpose
TBD - created by archiving change planilla-formula-point-mode. Update Purpose after archive.
## Requirements
### Requirement: Iniciar la edición de una celda

La planilla SHALL permitir entrar en modo edición de la celda activa de tres formas: doble click sobre la celda, presionar `Enter`, o empezar a escribir un carácter imprimible. Al empezar a escribir, el contenido previo de la celda SHALL reemplazarse por lo tipeado; al editar con doble click o `Enter`, SHALL mostrarse el contenido crudo existente (la fórmula, no su resultado) para editarlo.

#### Scenario: Editar con doble click muestra el contenido crudo
- **WHEN** una celda contiene la fórmula `=A1+A2` y el usuario hace doble click sobre ella
- **THEN** se abre el editor mostrando el texto `=A1+A2` (no su resultado)

#### Scenario: Empezar a escribir reemplaza el contenido
- **WHEN** una celda contiene `100` y el usuario, con la celda seleccionada, escribe `5`
- **THEN** se abre el editor con el contenido `5` (reemplaza el valor anterior)

### Requirement: Iniciar una fórmula con `=` o `+`

La planilla SHALL tratar como fórmula el contenido de una celda que empiece con `=`. Además, escribir `+` como primer carácter de una celda SHALL iniciar una fórmula equivalente a empezar con `=` (estilo planilla de cálculo: `+A1` se interpreta como `=A1`). Otros operadores iniciales no inician fórmula.

#### Scenario: `=` inicia fórmula
- **WHEN** el usuario escribe `=A1+A2` en una celda y confirma
- **THEN** la celda muestra el resultado de sumar `A1` y `A2`

#### Scenario: `+` como primer carácter inicia fórmula
- **WHEN** el usuario, con una celda seleccionada, escribe `+` como primer carácter
- **THEN** el editor entra en modo fórmula (el `+` inicial se interpreta como inicio de fórmula, equivalente a `=`)

#### Scenario: Texto que no es fórmula
- **WHEN** el usuario escribe `Total` en una celda
- **THEN** la celda guarda y muestra el texto `Total` sin evaluar

### Requirement: Construcción de fórmulas por referencia ("point mode")

Mientras se edita una celda cuyo contenido es una fórmula, mover la selección con las flechas o hacer click/arrastre sobre otra celda SHALL insertar la referencia de esa celda (`A1`, `B2`, o un rango `A1:B3` al arrastrar) en la posición del cursor dentro de la fórmula, en lugar de mover el caret de texto o cambiar la celda en edición. Movimientos consecutivos de apuntado SHALL reemplazar la referencia recién insertada (no acumularla), igual que en una planilla. Cuando el contenido NO es una fórmula, las flechas NO entran en point mode y se comportan según el requisito de confirmación y navegación.

#### Scenario: Flecha inserta referencia en una fórmula
- **WHEN** el usuario está editando una celda con el contenido `=` (cursor al final) y presiona la flecha hacia abajo una vez desde la celda `B2`
- **THEN** la fórmula pasa a `=B3` (se insertó la referencia de la celda apuntada)

#### Scenario: Apuntar consecutivo reemplaza la referencia tentativa
- **WHEN** tras lo anterior el usuario presiona la flecha hacia abajo otra vez
- **THEN** la fórmula pasa a `=B4` (reemplaza `B3` por la nueva celda apuntada, no `=B3B4`)

#### Scenario: Click sobre una celda inserta su referencia
- **WHEN** el usuario edita `=B1*` (cursor al final) y hace click en la celda `D5`
- **THEN** la fórmula pasa a `=B1*D5`

#### Scenario: Flechas en una celda de texto no entran en point mode
- **WHEN** el usuario edita la celda con el texto `Hola` (no empieza con `=` ni `+`) y presiona la flecha hacia abajo
- **THEN** se confirma `Hola` y la selección baja una celda (comportamiento de navegación normal)

### Requirement: Realimentación visual de la celda apuntada

Durante el point mode, la planilla SHALL resaltar visualmente la celda (o el rango) cuya referencia se está insertando, de forma distinta a la celda activa, para que el usuario sepa a dónde está apuntando. El resaltado SHALL actualizarse en cada movimiento de apuntado y SHALL desaparecer al confirmar, cancelar o al editar el texto manualmente (cuando la referencia tentativa deja de existir).

#### Scenario: La celda apuntada se resalta al navegar
- **WHEN** el usuario edita una fórmula y mueve el apuntado con flechas o mouse hacia la celda `B3`
- **THEN** la celda `B3` aparece resaltada (marcada como apuntada) mientras dura el point mode

#### Scenario: El resaltado sigue al apuntado y se limpia al confirmar
- **WHEN** el usuario mueve el apuntado de `B3` a `B4` y luego confirma con `Enter`
- **THEN** el resaltado pasa de `B3` a `B4` en cada movimiento y se quita al confirmar

### Requirement: Confirmar, cancelar y navegar al editar

Al editar una celda, la planilla SHALL confirmar el contenido y mover la selección de la siguiente manera: `Enter` confirma y baja una fila; `Tab` confirma y avanza a la derecha; `Shift+Tab` confirma y vuelve a la izquierda; `Escape` cancela la edición descartando los cambios. En point mode, `Enter`/`Tab`/`Shift+Tab` además fijan la referencia apuntada antes de confirmar.

#### Scenario: Shift+Tab vuelve a la celda anterior
- **WHEN** el usuario edita la celda `C2` y presiona `Shift+Tab`
- **THEN** se confirma el contenido y la celda activa pasa a `B2` (a la izquierda)

#### Scenario: Tab avanza a la derecha
- **WHEN** el usuario edita la celda `C2` y presiona `Tab`
- **THEN** se confirma el contenido y la celda activa pasa a `D2`

#### Scenario: Escape descarta cambios
- **WHEN** la celda `A1` contiene `10`, el usuario empieza a editar y escribe `99`, luego presiona `Escape`
- **THEN** la edición se cancela y `A1` sigue valiendo `10`

