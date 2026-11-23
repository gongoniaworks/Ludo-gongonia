# Plan de regresión de Ludo de Gongonia

## Cómo registrar una ejecución

Identificar commit y URL, navegador o versión de WebView, equipo físico o emulado, tamaño de ventana, orientación, participantes y modos. Por cada caso registrar la cobertura (`realizada`, `parcialmente realizada` o `pendiente`) y el resultado de los pasos ejecutados (`aprobado`, `fallido` o `no aplica`). Si hay un fallo, anotar los pasos para reproducirlo. Usar una sesión de prueba separada para las comprobaciones de persistencia.

### Cobertura registrada en la auditoría inicial

| Cobertura | Caso | Resultado observado |
|---|---|---|
| Realizada | Reproducción del fallo de bot en el sorteo, parte de `WEB-04`. | En horizontal, con una persona y un bot básico de Brunia, el turno del bot requirió pulsar el dado. |
| Realizada | Reproducción del fallo horizontal, parte de `WEB-10`. | Al comenzar la partida a `844 × 390`, Inicio interceptó los clics y el tablero quedó debajo. Al cambiar a `390 × 844` apareció la misma partida con el mismo turno observado antes del cambio. |
| Parcialmente realizada | `WEB-04` y `WEB-10` completos. | Quedan pendientes bot inicial, bot avanzado en el sorteo, desempates, cancelación, inicio en vertical, recorridos completos, cambios de orientación en otras fases y dispositivos físicos. |
| Pendiente | Demás casos completos de este plan. | Sin resultado registrado para el caso completo. Los hallazgos de lectura del código se distinguen de las observaciones en navegador en [ESTADO.md](ESTADO.md). |

### Cobertura por plataforma

- **Web.** Probar un escritorio y ventanas de referencia de `390 × 844` en vertical y `844 × 390` en horizontal. Verificar también una ventana horizontal cercana a la relación `1.15:1`. Repetir flujos críticos en Chrome para Android y Safari para iOS o iPadOS físicos y registrar las dimensiones reales.
- **Partidas.** Cubrir dos, tres y cuatro reinos. Incluir personas, persona con ambos tipos de bot y una mesa de bots para comprobar la automatización. En la versión actual las tiradas son aleatorias. Repetir los casos dependientes de resultados concretos, o usar una fuente controlada de resultados cuando el motor permita inyectarla.
- **Alcance futuro.** Los casos de Ajustes y créditos, publicidad, recuperación y Android son criterios de aceptación de sus respectivas fases. Repetir las regresiones web después de incorporar cada integración.

## Regresión de la web

| ID | Área | Acción y resultado esperado |
|---|---|---|
| WEB-01 | Inicio | Abrir Historia, Estadísticas, Compras y Ajustes. Comprobar que cada acceso abre la vista o el aviso correspondiente y que se puede cerrar o regresar a Inicio. En v0.12.0, elegir uno de los cinco idiomas cambia la etiqueta y el resto de la interfaz sigue en español. |
| WEB-01A | Compras superpuesta | Abrir Compras desde Inicio en ambas orientaciones. Verificar que aparece sobre Inicio y que los controles de fondo quedan fuera de la interacción mientras está abierta. Probar el botón de cierre y **Volver**. Al cerrar, Inicio vuelve a estar operativo y el foco regresa a Compras. |
| WEB-01B | Ajustes y créditos · futuro | Abrir Ajustes desde Inicio en ambas orientaciones. Comprobar que se puede leer la información de créditos en la misma vista y volver a Inicio desde ella. |
| WEB-02 | Consejo | Activar o vaciar plazas y alternar Persona, Bot básico y Bot avanzado. Probar con cero, uno, dos, tres y cuatro reinos. El botón de inicio se habilita desde dos reinos y el sorteo refleja exactamente la selección y los nombres. |
| WEB-03 | Sorteo | Con cuatro reinos A, B, C y D en ese orden, usar `6, 5, 4, 3` para verificar que A obtiene el primer turno. En otro sorteo usar `6, 6, 4, 3`, luego `5, 5` y por último `2, 4`, para que gane B. Solo A y B repiten, el historial conserva las tres rondas y **Comenzar partida** aparece después de resolver el empate. Verificar el primer turno en el tablero. |
| WEB-04 | Bots en el sorteo | Probar un bot básico como primer participante, una persona después y un bot avanzado a continuación. Forzar empate entre los dos bots. Tras la corrección, contar una tirada automática por bot en cada ronda en la que participa. El turno de la persona espera su entrada. Volver al Consejo antes de una tirada automática y durante otra tirada, y abrir un segundo sorteo. Las acciones del sorteo anterior no afectan al nuevo. |
| WEB-05 | Dados y turnos | Verificar valores de `1` a `6`, coherencia entre cara final y resultado aplicado y bloqueo de pulsaciones repetidas durante la tirada. Probar ausencia de movimiento con y sin tirada extra. Confirmar que el turno omite reinos ya finalizados. |
| WEB-06 | Bots en partida | Verificar que ambos tipos tiran y eligen fichas legales sin intervención. En posiciones controladas, comprobar que el avanzado favorece una llegada a meta frente a un avance ordinario. Repetir con tirada extra y sin fichas legales. |
| WEB-07 | Movimientos | Salir de base con `1` y `6` y comprobar que los demás valores no permiten salir. Probar avance por el anillo y el carril propio, llegada exacta y bloqueo al sobrepasar la meta. Confirmar que solo se resaltan fichas legales. |
| WEB-08 | Capturas | Caer sobre rivales en casilla segura y en casilla ordinaria. En la segura se conservan las fichas. En la ordinaria vuelven a sus bases y aumenta el contador de cada rival afectado. Si coinciden captura y `1` o `6`, comprobar que se concede una sola tirada extra. Durante la animación, las pulsaciones repetidas no deben adelantar el turno. |
| WEB-09 | Victoria | Completar cuatro fichas con un reino y verificar su celebración y que el juego prosigue si quedan más de un reino activo. Al final comprobar el ganador, las celebraciones y la pantalla de Resultados. El criterio exacto de clasificación del último reino sigue pendiente de definición. |
| WEB-10 | Orientación | Iniciar un recorrido completo en `390 × 844` y otro en `844 × 390`, sin girar, desde Inicio hasta Resultados. En recorridos separados, girar en ambos sentidos durante el sorteo con una tirada ya registrada y durante una partida a la espera de mover. Comprobar que se conservan ronda, historial, turno, dado y fichas legales según la fase. Inicio queda oculto fuera de su vista y el tablero y los botones activos reciben el toque. Repetir la transición Sorteo → Partida a ambos lados del umbral horizontal de `1.15:1`. |
| WEB-11 | Navegación | Probar Inicio ↔ Consejo, Consejo ↔ Sorteo, volver desde Partida y ambas salidas de Resultados. Cancelar y confirmar reinicio. Repetir durante la espera de un bot o una animación. Verificar foco, ausencia de acciones pendientes de la sesión abandonada y consistencia del siguiente sorteo. |
| WEB-12 | Estadísticas | Anotar los acumulados de partida, victorias, tiempo y participantes antes de jugar. Terminar y cotejar duración, tiradas, movimientos, capturas y participantes. Verificar el incremento de una partida y una victoria del reino correcto, y la suma del tiempo y participantes de esa partida. Volver a Inicio, abrir Estadísticas y recargar. Los valores se conservan sin otro incremento. Una partida abandonada no suma como completada. |
| WEB-13 | Audio | Probar primer gesto, cambio de pista al entrar y salir de Partida, volumen, silencio y efectos por separado. Simular una fuente externa de victoria inaccesible o usar una sesión sin conexión. La reproducción fallida no debe bloquear el flujo. |
| WEB-14 | Tacto y accesibilidad | Probar controles de Inicio y Consejo, dado y fichas legales al centro y en los bordes de sus zonas táctiles. Verificar alineación sobre el arte, zonas seguras, uso con teclado en web y nombres accesibles de los controles. |

## Publicidad · cobertura futura

Para `PUB-02` a `PUB-05`, comprobar con un proveedor de prueba que el resultado ya esté guardado cuando se solicita el anuncio. Al abrir Resultados, volver a Inicio y recargar, los acumulados deben reflejar una sola finalización. Probar también una respuesta duplicada o tardía del proveedor y verificar que no vuelve a abrir el anuncio ni aumenta los acumulados.

| ID | Escenario | Resultado esperado |
|---|---|---|
| PUB-01 | Cargar las vistas con espacios publicitarios en vertical y horizontal, con y sin anuncio. | El espacio se adapta sin cubrir el tablero ni los controles. Se conservan la visibilidad y la interacción por toque. |
| PUB-02 | El anuncio está disponible y termina correctamente. | Se guarda el resultado, se muestra el anuncio y al finalizar se abren las estadísticas finales. |
| PUB-03 | El proveedor indica que no hay anuncio disponible. | Se abre Resultados en cuanto se conoce la falta de anuncio. |
| PUB-04 | El proveedor devuelve un error al cargar o mostrar el anuncio. | Se abre Resultados y se puede navegar desde allí. |
| PUB-05 | El proveedor no responde o el anuncio queda abierto sin emitir el evento de finalización. | Al alcanzar el tiempo máximo configurado se abre Resultados. Las respuestas posteriores no alteran la vista ni el registro. El valor exacto del límite queda pendiente de definición. |

## Recuperación de partidas · cobertura futura

| ID | Escenario | Resultado esperado |
|---|---|---|
| REC-01 | Recargar antes de tirar, durante un sorteo con historial y después de una tirada con una ficha aún por elegir. En este último caso, preparar una posición con fichas legales e ilegales y registrar cuáles están habilitadas. | Se recuperan la fase, el tablero, la ronda y el historial del sorteo o el turno de la partida, según corresponda. Si había una tirada pendiente, se conservan su valor y sus contadores. Se habilitan exactamente las mismas fichas legales y se puede ejecutar una de ellas sin volver a lanzar el dado. |
| REC-02 | Cerrar o suspender durante una animación o mientras espera un bot. | La restauración usa el último punto consistente y produce una única acción al reanudar el bot. |
| REC-03 | Abrir con guardado corrupto o de una versión anterior y con estadísticas preexistentes. | Se aplica la migración prevista o se permite empezar una sesión nueva si la recuperación no es posible. Los acumulados previos se conservan. |
| REC-04 | Cerrar tras finalizar y volver a abrir. | El mismo identificador de partida no duplica victorias, tiempo ni recuento de partidas. |

## Android y distribución · cobertura futura

| ID | Escenario | Resultado esperado |
|---|---|---|
| AND-01 | Ejecutar en Android físico y emulador. Girar, suspender, reanudar, cerrar el proceso y usar Atrás en las vistas principales. | Navegación, zonas seguras y recuperación coherentes. El cambio de ciclo de vida no duplica acciones ni audio. |
| AND-02 | Arrancar y jugar sin conexión, con retorno al primer plano y cambios de audio. | Los recursos locales permiten entrar y jugar. Si una integración futura depende de red, su ausencia no bloquea el turno. |
| AND-03 | Generar el AAB firmado y validarlo mediante una entrega de prueba autorizada en Google Play. | El paquete, su identidad y requisitos de publicación vigentes quedan verificados. La web de Cloudflare supera en paralelo sus regresiones de entrada y partida. |
