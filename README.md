# Ludo de Gongonia — v0.12.0 (base de desarrollo)

Este repositorio contiene la **línea de desarrollo nueva** de Ludo de Gongonia. Se trabaja sobre una sola rama principal, `main`.

## Procedencia verificada

- Repositorio histórico: [Ludo-Gongonia-Seguridad](https://github.com/gongoniaworks/Ludo-Gongonia-Seguridad).
- Rama de referencia: `v0-11-4-dado-marfil-vertical`.
- Commit de origen: `e33b68b7cb28ed92ce8d87757ec0665401d7be0e`.
- Se copiaron sin modificar **27 archivos de juego y recursos**, incluidos `index.html`, imágenes y audio. Sus SHA de Git coinciden con la referencia histórica.
- Este README es la única adaptación inicial de documentación.

**Estado:** importación completada y verificada a nivel de archivos. Aún no es una versión estable aprobada ni se han superado las pruebas visuales y táctiles en dispositivos.

**Importante:** el código y los textos visibles dentro de `index.html` siguen correspondiendo a la versión heredada. La numeración v0.12.0 identifica el nuevo punto de partida en GitHub, no una modificación funcional.

## Incidencias conocidas

1. En modo horizontal, después del sorteo inicial, se regresa a la pantalla de inicio en vez de comenzar la partida.
2. Durante el sorteo inicial, los bots esperan que una persona lance el dado; durante la partida ya actúan automáticamente.
3. Ajustes visuales pendientes de inventario.

## Política de trabajo

- No modificar el repositorio histórico.
- Realizar cambios pequeños y verificables en `main`, con commits identificables y reversibles.
- Ejecutar pruebas de regresión para vertical y horizontal.
- No declarar estable ninguna versión sin pruebas suficientes y aprobación del usuario.
- Mantener por separado el despliegue de pruebas respecto del juego histórico.

## Publicación

Cloudflare Pages: `ludo-gongonia.pages.dev`, conectado a la rama `main` del repositorio nuevo. La conexión histórica `ludo-gongonia-preview` permanece separada.
