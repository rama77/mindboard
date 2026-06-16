## Why

La planilla de cálculo de la tarjeta ya permite escribir fórmulas, pero **armarlas no se siente como una planilla**: al editar una celda y empezar una fórmula, las flechas mueven el cursor del texto en vez de dejar elegir celdas, y no hay forma de "apuntar" a una celda para insertar su referencia. Además, **Shift+Tab no vuelve a la celda anterior** (siempre avanza). Esto rompe la expectativa de cualquiera que viene de Excel/Sheets y hace que crear fórmulas sea incómodo.

## What Changes

- **"Point mode" al editar una fórmula.** Cuando la celda en edición contiene una fórmula (empieza con `=`), mover con las flechas o clickear/arrastrar sobre otras celdas **inserta o actualiza la referencia** (`A1`, `B2`, rango `A1:B3`) en la posición del cursor, en lugar de mover el caret o cambiar de celda. `Enter`/`Tab` confirman fijando la referencia apuntada; `Escape` cancela la edición.
- **`+` inicia fórmula.** Escribir `+` como primer carácter de una celda arranca una fórmula igual que `=` (estilo Excel: `+A1` ⇒ `=A1`). `=` sigue funcionando igual.
- **Edición de texto sin fórmula sin cambios.** Si el contenido NO es una fórmula, las flechas confirman y navegan como hoy.
- **Fix `Shift+Tab`.** Al confirmar con `Tab` se avanza a la derecha; con `Shift+Tab` se vuelve a la izquierda.

## Capabilities

### New Capabilities
- `planilla-calculo`: edición de celdas y entrada de fórmulas en la planilla de la tarjeta — cómo se inicia y confirma la edición, navegación entre celdas al editar, y construcción de fórmulas por referencia ("point mode"). Formaliza por primera vez en spec el comportamiento de edición (la planilla se implementó sin spec previa) y captura las mejoras de esta propuesta.

### Modified Capabilities
<!-- Ninguna: no hay specs existentes cuyos requisitos cambien. -->

## Impact

- **Código:** `index.html`, sección JS de la planilla (`ssBeginEdit`, `ssKeydown`, `ssSelect`, wiring de teclado y del `<input>` de edición). Solo lógica de UI/teclado.
- **Modelo de datos:** sin cambios (las fórmulas se siguen guardando como string crudo en `task.sheet.cells[ref].v`, en localStorage).
- **Sin dependencias nuevas**, sigue siendo un único `index.html` vanilla, offline.
- **Sin breaking changes**: las fórmulas ya guardadas siguen evaluándose igual.
