# Decisiones técnicas

## Aprobadas

| Fecha de registro | Decisión | Motivo y alcance |
|---|---|---|
| 2026-11-22 | Trabajar en `gongoniaworks/Ludo-gongonia`, rama principal `main`. | Mantiene una línea de desarrollo identificable separada del repositorio histórico `gongoniaworks/Ludo-Gongonia-Seguridad`, reservado para consulta. |
| 2026-11-22 | Usar `v0.12.0` como base funcionalmente idéntica a `v0.11.4`. | Da a las correcciones una referencia reproducible. El identificador de la base no equivale a una declaración de estabilidad. |
| 2026-11-22 | Priorizar estabilidad, diagnóstico y regresión. | Los cambios pequeños y reversibles facilitan aislar defectos y conservar el comportamiento, los recursos visuales aprobados y las zonas táctiles. Una versión se declara estable tras pruebas suficientes y aprobación del usuario. |
| 2026-11-22 | Limitar esta fase a documentación local. | Permite revisar reglas de trabajo, arquitectura, estado y pruebas antes de modificar el juego. La fase excluye refactorización, instalación de dependencias, commits, push, merges y despliegues. Publicar estos documentos requiere autorización del usuario. |
| 2026-11-22 | Publicar la v1.0 en Google Play mediante AAB y mantener la versión web en Cloudflare Pages. | Conserva el acceso desde navegador y fija el formato de distribución de Android. Capacitor es la integración futura prevista. Su instalación y configuración quedan fuera de esta fase. |
| 2026-11-22 | Mantener nueve vistas funcionales previstas, con Ajustes y créditos juntos por ahora y Compras como ventana superpuesta a Inicio. | Conserva una navegación clara y el retorno directo a Inicio al cerrar Compras. Una separación posterior de Ajustes y créditos se evaluará si hace falta. |
| 2026-11-22 | Prever espacios publicitarios adaptables y mantener el tablero y sus controles visibles y operables. | Protege el espacio de juego en vertical y horizontal. |
| 2026-11-22 | Guardar el resultado una sola vez al terminar. Después, intentar el anuncio y mostrar las estadísticas finales. | El resultado se guarda antes del intento. Si el anuncio termina, falla, no está disponible o agota el tiempo máximo, se muestran los resultados. |
| 2026-11-22 | Mantener la recuperación de partidas, la traducción a los cinco idiomas previstos y la integración con Capacitor como alcance futuro. | Su aprobación documental fija los objetivos. Esta fase autoriza únicamente cambios en la documentación local. |

## Pendientes de definición

- Criterio exacto de clasificación del último reino que aún tenga fichas fuera de meta.
- Ubicación exacta de los espacios publicitarios, proveedor, gestión del consentimiento y valor del tiempo máximo de espera.
- Esquema y migración del guardado, organización de los catálogos de traducción, dependencias para Android e integración de compras, incluido su catálogo y flujo de pago.

La secuencia modular de [ARQUITECTURA.md](ARQUITECTURA.md) queda como propuesta. Los acuerdos de alcance anteriores sirven de referencia para una futura autorización de implementación.
