# Guía para agentes de IA (y humanos) — mindboard

> Referencia para asistentes de IA (Claude, Cursor, Copilot, Codex, etc.) y para quien
> contribuya. `CLAUDE.md` apunta a este archivo.

## Qué es

mindboard es un tablero kanban **en un solo archivo** `index.html`: HTML + CSS + JavaScript
**vanilla**. Sin build, sin frameworks, sin dependencias, sin CDN. Se abre con doble clic
(`file://`) y funciona offline.

## Reglas de oro (no romper)

- **Todo vive en `index.html`.** El `<style>` y el `<script>` van inline en ese archivo.
  No agregar JS/CSS externos, paquetes npm, ni `<script src>` a CDNs.
- **Los datos del usuario viven en `localStorage`**, nunca dentro del HTML. No hardcodear
  tareas/datos de ejemplo en el markup ni incrustar info del usuario.
- **Offline-first**: nada de llamadas de red en el camino crítico.
- **Colores por tokens semánticos** (variables CSS: `--foreground`, `--card`, `--border`,
  `--brand-teal`, `--danger`, etc.). No hardcodear colores hex sueltos.
- El prefijo de clases `ap-` es legacy (significa "app"); mantenelo por consistencia.

## Cómo correr / probar

- Abrí `index.html` en el navegador (doble clic). No hay servidor ni build.
- La **consola debe quedar limpia** (cero errores).
- Chequeo rápido de sintaxis JS sin navegador: extraé el contenido de `<script>` y corré
  `node --check`. (No reemplaza probar en el navegador.)

## Estructura (dentro de index.html)

- Un IIFE con el estado en memoria + helpers `read`/`write` a `localStorage`
  (claves `mindboard*.v1`).
- `render()` redibuja el tablero; hay `renderSidebar()`, `renderItemsView()`, etc.
- Modales por sección de una tarjeta: editor, mapa mental, pizarra, ítems (control
  segmentado arriba).

## Gotchas conocidos

- `display` por CSS le gana al atributo `hidden`: para mostrar/ocultar contenedores con
  `display` propio usá `style.display`, no `el.hidden`.
- El drag & drop está aislado por tipo (tarjetas / carriles / nodos del mapa / formas de la
  pizarra); al tocar uno, no romper los otros.
- Al exportar la pizarra a imagen se clona el SVG y se copian estilos computados: mantené el
  clon alineado 1:1 con el original antes de quitar elementos.

## Flujo de contribución

- `main` está **protegida**: no se pushea directo. Los cambios entran por **Pull Request**
  con al menos una aprobación del/los mantenedor/es.
- Cambios acotados y enfocados (un PR, un tema). Mantené el estilo vanilla existente.
- Detalle en `CONTRIBUTING.md`.

## Cambios con OpenSpec

Este repo usa [OpenSpec](https://github.com/Fission-AI/OpenSpec) para cambios no triviales
(features, refactors, fixes multi-archivo): se propone un cambio con `/opsx:propose`, se
revisa la spec y recién se implementa. Las specs viven en `openspec/`. Para typos o fixes
de una línea no hace falta.
