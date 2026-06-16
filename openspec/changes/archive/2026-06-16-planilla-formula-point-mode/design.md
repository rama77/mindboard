## Context

La planilla (sección "Planilla" de la tarjeta) ya tiene edición de celdas y motor de fórmulas (`ssCompute`), pero la edición es un `<input>` plano: las flechas mueven el caret del texto y no hay forma de "apuntar" a celdas. El editor se crea en `ssBeginEdit(initial)` y maneja sus propias teclas (`Enter`, `Tab`, `Escape`) con `stopPropagation`, por lo que la navegación con flechas de `ssKeydown` no aplica mientras se edita. La selección de celdas vive en `ssActive`/`ssAnchor`/`ssRange` y se dibuja en `renderSheet()`.

Restricciones: un solo `index.html` vanilla, sin dependencias, datos en localStorage. El modelo de datos no cambia (la fórmula es el string crudo en `cells[ref].v`).

## Goals / Non-Goals

**Goals:**
- Que armar una fórmula con flechas o click se sienta como Excel/Sheets ("point mode").
- `+` inicial inicia fórmula.
- `Shift+Tab` vuelve a la celda anterior.
- No regresionar la edición de texto plano ni el resto de la planilla.

**Non-Goals:**
- Autocompletar nombres de funciones o resaltar referencias con colores.
- Selección de rangos por teclado dentro del point mode con `Shift+flechas` (solo arrastre con mouse inserta rango). Se puede sumar después.
- Edición multi-celda o referencias a otras tarjetas/hojas.

## Decisions

### Detección de "modo fórmula" durante la edición
El editor entra en point mode cuando su valor empieza con `=`. Para el atajo de inicio con `+`: en `ssBeginEdit`, si `initial === "+"`, se siembra el editor con `"="` en lugar de `"+"` (así `+A1` ⇒ `=A1`). No se reinterpretan operadores en medio del texto: solo el primer carácter.

**Alternativa descartada:** permitir que cualquier operador inicial (`-`, `*`) inicie fórmula. Excel solo trata `=` y `+` (y `-`) como inicio; para mantenerlo simple y predecible, solo `=` y `+`.

### Point mode: flechas y click insertan referencia
Se mantiene un estado de edición `ssEdit = { pointing: bool, refStart: int, refEnd: int }` mientras el editor está abierto:
- El editor escucha `keydown` de flechas. Si el valor empieza con `=`, se hace `preventDefault()` y, en vez de mover el caret, se calcula la celda apuntada a partir de una posición de apuntado interna (`ssPoint = {r,c}`, inicializada en la celda en edición) desplazada por la flecha, acotada al rango de la hoja.
- La referencia apuntada se inserta en el texto en la posición del cursor. Si el apuntado anterior ya había insertado una referencia tentativa (`refStart..refEnd`), se reemplaza ese tramo; si no, se inserta en el caret y se registra el tramo. Así los movimientos consecutivos reemplazan en vez de acumular.
- Click/arrastre sobre celdas: el handler de `mousedown`/`mouseover` de la tabla, cuando `ssEditing && valorEmpiezaCon "="`, inserta la referencia (o rango, si se arrastra) en vez de cambiar `ssActive`. Tras soltar, el foco vuelve al editor.
- `Enter`/`Tab`/`Shift+Tab` "fijan" la referencia tentativa (dejan de tratarla como reemplazable) y luego confirman y navegan.

**Alternativa descartada:** parsear la fórmula para encontrar la última referencia. Es frágil; el enfoque de "tramo tentativo" (`refStart/refEnd`) es local y robusto.

### Navegación y confirmación
`ssBeginEdit` ya maneja `Enter` (baja) y `Tab` (derecha). Se agrega `Shift+Tab` (izquierda) leyendo `e.shiftKey` en la rama de `Tab`. La confirmación sigue usando `ssCommitEdit` + `ssSelect` + `renderSheet`.

### Aislamiento del comportamiento
Toda la lógica nueva vive dentro del editor (`ssBeginEdit`) y en los handlers de tabla ya existentes, gateada por "el valor empieza con `=`". Si no es fórmula, el camino actual queda intacto (las flechas no se interceptan en el `<input>`, así que el caret se mueve normal; para confirmar-y-navegar con flechas en texto plano se mantiene el comportamiento actual).

## Risks / Trade-offs

- **[Reemplazo de referencia tentativa mal calculado al editar a mano]** → El tramo `refStart/refEnd` se invalida en cuanto el usuario tipea o mueve el caret manualmente (cualquier `input`/`keydown` no-flecha resetea `pointing`), evitando reemplazos sorpresivos.
- **[Click en point mode vs. cerrar el editor por blur]** → El `mousedown` sobre la tabla durante edición se maneja con `preventDefault` para no robar el foco y dispara la inserción; el commit por `blur` se evita mientras `pointing` está activo.
- **[Confusión entre apuntar y navegar]** → Mitigado porque point mode solo se activa con `=`/`+`; el usuario que escribe texto nunca lo ve.
- **Riesgo bajo en general:** cambios localizados de UI, sin tocar el motor de cálculo ni el modelo de datos; fácil de revertir.
