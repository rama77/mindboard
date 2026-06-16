## 1. Inicio de fórmula con `+` y edición

- [x] 1.1 En `ssBeginEdit(initial)`: si `initial === "+"`, sembrar el editor con `"="` (así `+A1` ⇒ `=A1`). Verificar que escribir `+` sobre una celda abre el editor en modo fórmula.
- [x] 1.2 Confirmar que doble click / `Enter` siguen mostrando el contenido crudo y que tipear reemplaza (sin regresión).

## 2. Fix de navegación al confirmar

- [x] 2.1 En el handler de teclas del editor, manejar `Shift+Tab`: confirmar y mover a la izquierda (`ssSelect(r, c-1)`), separado del `Tab` que mueve a la derecha (`ssSelect(r, c+1)`).
- [x] 2.2 Verificar `Enter` (baja una fila) y `Escape` (descarta cambios) siguen funcionando.

## 3. Point mode (flechas)

- [x] 3.1 Agregar estado de edición para point mode: posición apuntada (`ssPoint = {r,c}`), flag `pointing`, y tramo de la referencia tentativa (`refStart`/`refEnd`) sobre el valor del editor.
- [x] 3.2 En el `keydown` del editor, si el valor empieza con `=`, interceptar flechas (`preventDefault`): desplazar `ssPoint` acotado a la hoja e insertar/reemplazar la referencia tentativa en la posición del cursor.
- [x] 3.3 Resetear `pointing`/tramo tentativo ante cualquier edición manual (tecla imprimible, Backspace, o movimiento de caret con Home/End/click dentro del input).
- [x] 3.4 En `Enter`/`Tab`/`Shift+Tab` dentro de point mode: fijar la referencia tentativa antes de confirmar y navegar.

## 4. Point mode (mouse)

- [x] 4.1 En el `mousedown` de la tabla: si `ssEditing` y el valor del editor empieza con `=`, `preventDefault` (no robar foco), insertar la referencia de la celda clickeada en el cursor y devolver foco al editor, en vez de cambiar `ssActive`.
- [x] 4.2 En `mouseover` durante arrastre en ese estado: actualizar a un rango `A1:B3` reemplazando la referencia tentativa.
- [x] 4.3 Evitar que el `blur` del editor confirme mientras se está apuntando con el mouse.

## 5. Realimentación visual de la celda apuntada

- [x] 5.0a CSS `.ss-cell.ss-point` (resaltado distinto a `.sel`, con tokens semánticos).
- [x] 5.0b `ssPaintPoint(r1,c1,r2,c2)`/`ssClearPointPaint()` que togglean la clase sin re-renderizar (para no destruir el editor); llamarlas desde `ssPointMove` (teclado) y los handlers de mouse (click = celda, arrastre = rango). Limpiar en `ssClearPoint`.

## 6. Verificación

- [x] 5.1 `node --check` del `<script>` (consola limpia, regla del proyecto).
- [x] 5.2 Probar en el navegador cada scenario de la spec: `+` inicia fórmula, flecha inserta y reemplaza referencia, click/arrastre insertan ref/rango, texto plano navega normal, `Shift+Tab` vuelve a la izquierda, `Escape` descarta.
- [x] 5.3 Actualizar `CHANGELOG.md` (sección No publicado) con la mejora de fórmulas y el fix de Shift+Tab.
