# Estado del proyecto · 22 de noviembre de 2026

## Base y entornos

- **Versión de desarrollo** `v0.12.0`, funcionalmente idéntica a `v0.11.4`. Línea base auditada en `main`, commit `ee4096f1bc0d0c6c099d048214930cdcef2671ce`. La aprobación de estabilidad está pendiente. [GitHub activo](https://github.com/gongoniaworks/Ludo-gongonia/tree/main)
- **Web activa** Cloudflare Pages. El título de la página publicada muestra `v0.11.0` y el pie horizontal muestra `v0.11.3`, ambos rótulos heredados. Falta relacionar el identificador del despliegue con el commit exacto. [Juego publicado](https://ludo-gongonia.pages.dev)
- **Referencia histórica de solo consulta** El `index.html` auditado coincide por SHA de archivo con el de `e33b68b7cb28ed92ce8d87757ec0665401d7be0e`. [Repositorio histórico](https://github.com/gongoniaworks/Ludo-Gongonia-Seguridad) · [Cloudflare histórico](https://ludo-gongonia-preview.pages.dev)

## Problemas conocidos y resultados comprobados

| Prioridad | Evidencia de auditoría | Estado |
|---|---|---|
| P0 | Tras pulsar **Comenzar partida** a `844 × 390`, el tablero se inicializó pero Inicio quedó por encima y recibió los clics. En CSS horizontal, `#homeScreen.home-screen` fuerza `display:block!important` y `z-index:100` pese a `hidden`. Al pasar a `390 × 844` apareció la misma partida con el mismo turno observado antes de girar. | Reproducido en la web publicada y explicado en código. |
| P1 | En el sorteo de dos reinos, el bot básico de Brunia esperó la pulsación manual. El sorteo depende del botón, mientras la partida tiene su propio planificador de bots. | Reproducido en la web publicada y explicado en código. |
| P2 | El aviso inicial de Partida pide `6`, mientras la regla admite `1` o `6` para salir. Resultados puede asignar un puesto al último reino aunque aún tenga fichas fuera de meta. | Comprobado en código. Falta validar el cierre completo en ejecución y definir el criterio exacto de clasificación del último reino. |

## Comportamiento actual y alcance futuro

| Área | Comportamiento actual en v0.12.0 | Pendiente de implementación |
|---|---|---|
| Ajustes y Compras | Ambos accesos reutilizan un diálogo informativo. La marca y la versión están en el pie horizontal de Inicio. | Reunir Ajustes y créditos en una vista. Conservar Compras como ventana superpuesta a Inicio al preparar la tienda. |
| Publicidad | Inicio horizontal tiene un hueco visual. El cierre actual muestra Resultados después de la celebración de victoria. | Incorporar espacios adaptables que dejen libres tablero y controles. Guardar el resultado, intentar el anuncio y mostrar las estadísticas finales, incluso ante error, ausencia de anuncio o tiempo agotado. |
| Recuperación | El estado de partida está en memoria. El navegador guarda únicamente estadísticas agregadas de partidas terminadas. | Guardar y restaurar la sesión, incluida la ronda de sorteo o la tirada pendiente y sus movimientos legales. Conservar los acumulados y registrar una sola finalización por partida. |
| Idiomas | Cinco opciones en el selector. Los textos del juego permanecen en español. | Traducir la interfaz y conservar la preferencia. |
| Android | La versión actual se ejecuta como web estática. | Integrar Capacitor y preparar el AAB de v1.0 para Google Play, manteniendo la web de Cloudflare. |

## Riesgos y trabajo pendiente

- **Temporizadores y concurrencia.** El análisis encontró esperas de tiradas, capturas y cambios de turno que podrían continuar tras abandonar una vista o reiniciar. Es un riesgo inferido del código. Falta reproducirlo con entradas rápidas y cambios de sesión.
- **Persistencia.** Se silencian los errores de escritura de los acumulados. Quedan pendientes la migración y la validación de los acumulados tras recarga.
- **Orientación y tacto.** La prueba dinámica cubrió una partida de dos reinos con bot básico y tamaños emulados. Falta probar dispositivos físicos, comenzar en vertical, girar en distintas fases, completar una partida y revisar ítems visuales pendientes. También faltan bot avanzado y desempates forzados.
- **Recursos y audio.** Falta inventariar variantes de imágenes y comprobar tamaño, alineación táctil, disponibilidad sin conexión y licencias de distribución del audio, incluidos los efectos remotos.
- **Siguiente fase.** Priorizar las correcciones P0 y P1 tras autorización. Ejecutar las regresiones de [PRUEBAS.md](PRUEBAS.md). Después evaluar la extracción modular y la recuperación, seguidas de los cinco idiomas y la preparación de publicidad, compras y Capacitor/AAB. [Arquitectura objetivo](ARQUITECTURA.md)
