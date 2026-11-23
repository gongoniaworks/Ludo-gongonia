# Arquitectura de Ludo de Gongonia

## Arquitectura actual · v0.12.0

`v0.12.0` identifica la base de desarrollo del repositorio activo y conserva el comportamiento de `v0.11.4`. El juego es una web estática. [`index.html`](../index.html) concentra el marcado, el CSS adaptable y un bloque de JavaScript que controla el juego y la interfaz. La raíz y `assets/` contienen imágenes, variantes y audio. Los dos archivos `prueba-inicio-v0101*.html` son prototipos separados. El árbol actual carece de dependencias npm, suite automatizada y proyecto Android.

### Nueve vistas funcionales

El diseño prevé nueve vistas funcionales. La versión actual las distribuye en seis secciones HTML y tres accesos modales. El Sorteo se presenta sobre el Consejo. Estadísticas tiene su propio diálogo, mientras Compras y Ajustes reutilizan el mismo diálogo informativo de Inicio. El selector de idiomas, el aviso de saldo GNG y la celebración de victoria son capas auxiliares del flujo.

| Vista | Implementación y navegación actual |
|---|---|
| Inicio | `homeScreen` enlaza con el Consejo, la Historia y tres accesos modales. Incluye sonido, selector de idioma y aviso de saldo GNG. |
| Historia | `storyScreen` muestra el origen de los Juegos y permite volver a Inicio. |
| Consejo de los Cuatro Reinos | `setup` configura de dos a cuatro reinos, nombres y modos. Abre el Sorteo o vuelve a Inicio. |
| Sorteo de salida | `drawScreen` se superpone al Consejo. Permite volver a la configuración o abrir Partida después de determinar quién empieza. |
| Partida | `game` contiene tablero, dado, turno y fichas. Permite regresar al Consejo o reiniciar, previa confirmación, con el mismo reino inicial. |
| Resultados | `results` muestra duración, clasificación y detalle. Permite iniciar otro sorteo con la selección actual o regresar a Inicio. |
| Estadísticas | Modal de Inicio que muestra acumulados locales. |
| Compras | Ventana superpuesta a Inicio que muestra un aviso en el diálogo informativo reutilizado. La tienda se encuentra pendiente. |
| Ajustes y créditos | Ajustes abre un aviso en el diálogo informativo. La marca y la versión aparecen en el pie horizontal de Inicio. Los controles de música, efectos e idioma viven en las vistas existentes. Reunir Ajustes y créditos es parte del diseño objetivo. |

La navegación modifica clases como `hidden` desde los manejadores de cada vista. La celebración de victoria es una capa sobre el juego. En horizontal, la cascada de CSS puede mantener Inicio por encima de otras vistas. El diagnóstico y la evidencia se detallan en [ESTADO.md](ESTADO.md).

### Motor, dados y bots

- El sorteo asigna el primer turno al mayor valor de un dado de seis caras. Solo los reinos empatados en el máximo repiten. Los modos son persona, bot básico, bot avanzado y plaza vacía. En el código actual, incluso un turno de bot en el sorteo espera el botón.
- Cada reino usa cuatro fichas. Las posiciones se calculan sobre un anillo de 52 casillas con ocho seguras y un carril final de cinco casillas por reino. Se sale de base con `1` o `6` y se llega a meta con la tirada exacta. Una captura fuera de casilla segura devuelve las fichas rivales afectadas a la base. Sacar `1` o `6`, capturar o llegar a meta puede dar una tirada extra. Si coinciden varios motivos en un movimiento, el beneficio se concede una vez.
- Durante la partida el bot básico elige al azar entre las fichas legales. El avanzado puntúa llegada, capturas, salida de base y avance. Las tiradas usan `Math.random()`. El planificador, el DOM y las animaciones comparten el estado mutable de la sesión. El siguiente turno recorre el orden de reinos activos y omite los que han terminado.

### Estado y estadísticas

La configuración se lee del formulario. El sorteo, las fichas, el turno, el dado, los contadores detallados y las preferencias de sonido viven en memoria. Una recarga pierde la sesión. `localStorage`, con la clave `ludoGongoniaLocalStatsV1`, conserva los acumulados de partidas terminadas por origen y perfil de navegador. Los acumulados incluyen número de partidas, tiempo, participantes y victorias por reino. Resultados presenta tiradas, movimientos y capturas por rival, además de la clasificación de esa partida.

### Recursos y funciones preparadas

| Área | Situación actual |
|---|---|
| Gráficos | Inicio combina ilustración y zonas táctiles en vertical y recortes de la ilustración en horizontal. El Consejo usa una imagen de referencia. El tablero posiciona fichas WebP con coordenadas porcentuales. Los dados usan SVG en línea. |
| Música y efectos | Las pistas locales son *The Kelta Mountains* y *The Routa Plains of Ánganor (Take 2)*. Hay efectos sintetizados con Web Audio y dos efectos de victoria remotos. La música depende de la autorización de reproducción del navegador tras una interacción. |
| Idiomas | El selector ofrece español, italiano, inglés, alemán y francés. La elección cambia la etiqueta visible. Los textos del juego permanecen en español. |
| Publicidad | Hay un espacio visual en Inicio horizontal. El juego carece de integración con un proveedor de anuncios. |
| Compras | El aviso de Compras y el saldo gráfico de `0 GNG` son elementos de interfaz. La tienda, el saldo operativo y las transacciones están pendientes. |

## Arquitectura objetivo · propuesta gradual

La secuencia de implementación es una propuesta. Las decisiones de alcance aprobadas y las cuestiones pendientes figuran en [DECISIONES.md](DECISIONES.md). La recuperación de partidas, la traducción de la interfaz y la integración con Capacitor son trabajos futuros.

| Etapa | Diseño propuesto |
|---|---|
| 1. Estabilizar la versión web | Corregir la visibilidad por orientación y la tirada de bots en el sorteo con cambios localizados. Fijar el comportamiento esperado con las pruebas críticas de navegación y turnos. |
| 2. Separar reglas e interfaz | Extraer el tablero lógico, las reglas y la estrategia de bots a módulos JavaScript. Recibir los resultados del dado mediante una fuente controlable en pruebas. Una máquina de estados coordinaría Sorteo, Partida y Resultados. La interfaz gestionaría gestos, renderizado y animaciones. Cancelar operaciones pendientes al abandonar o reiniciar una sesión. |
| 3. Recuperar partidas y conservar estadísticas | Guardar una sesión versionada con participantes, fase, ronda si aplica, tablero, turno, tirada pendiente y contadores. Guardar al completar transiciones de estado y restaurar desde un punto consistente. Conservar los acumulados existentes y usar un identificador de partida para contabilizar cada finalización una sola vez. |
| 4. Internacionalizar y preparar servicios | Extraer textos y etiquetas accesibles para los cinco idiomas, guardar la preferencia y revisar texto incrustado en el arte. Mantener la publicidad y las compras en la interfaz, fuera del motor de reglas. |
| 5. Empaquetar para Android | Usar Capacitor con la salida web y los recursos locales. Integrar Atrás, suspensión y reanudación con la navegación y el guardado. Validar audio, zonas seguras y rotación en WebView. La v1.0 se publicará en Google Play mediante Android App Bundle (AAB). Se mantendrá en paralelo la versión web en Cloudflare Pages. |

### Presentación y publicidad previstas

- **Ajustes y créditos.** Mantener ambos contenidos en una misma vista por ahora. Una separación futura queda sujeta a revisión.
- **Compras.** Mantenerla como ventana superpuesta a Inicio. Al cerrarla, la persona vuelve a Inicio.
- **Espacios publicitarios.** Prever espacios que se adapten a la vista, al tamaño y a la orientación. Reservar espacio fuera del tablero y de los controles para mantenerlos visibles y operables.
- **Cierre de partida.** El flujo previsto es **fin de partida → guardar resultado → intentar anuncio → mostrar estadísticas finales**. Registrar la finalización una sola vez por partida antes de solicitar el anuncio. Si el anuncio termina, falla, no está disponible o alcanza el tiempo máximo de espera, mostrar Resultados. Ignorar respuestas publicitarias tardías tras abrir Resultados. El valor del tiempo máximo y la ubicación exacta de los espacios quedan pendientes.
