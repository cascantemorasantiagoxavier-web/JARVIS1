---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-001: Separar la lógica en `:core` (Kotlin/JVM puro) y `:app` (Android)

## Contexto
El entorno de desarrollo no puede descargar el Android Gradle Plugin ni AndroidX (ver ADR-004), y
además la lógica del asistente (cerebro, herramientas, Realtime, memoria) debe probarse rápido y sin
emulador.

## Decisión
- `:core` contiene toda la lógica sin dependencias de Android: cerebro, intenciones, 33
  herramientas, controlador Realtime, memoria, notas, tareas, negocios, globo, cliente del backend.
- `:app` solo implementa interfaces de `:core` (repositorios Room, `DeviceGateway`,
  `RealtimeTransport`…) y la UI.
- `-Pjarvis.coreOnly=true` excluye `:app` y `google()` del build.
- Inyección de dependencias manual (`AppContainer`) en vez de Hilt: menos dependencias de Google
  Maven y sin procesadores de anotaciones extra.

## Consecuencias
+ 67 pruebas de la lógica se ejecutan en cualquier máquina con JDK 17.
+ La misma lógica serviría para otras plataformas.
− Un poco más de código de interfaces y adaptadores.
