---
tipo: adr
estado: Aceptada (temporal)
fecha: 2026-10-23
---
# ADR-004: Entorno de desarrollo sin Google Maven: versiones de AndroidX sin verificar

## Contexto
La política de red del entorno donde se construyó el proyecto bloquea `dl.google.com` (al que
redirige `maven.google.com`), `api.openai.com`, `github.com`, la documentación de OpenAI, Overpass,
Nominatim, Wikipedia y DuckDuckGo. Maven Central, el portal de plugins de Gradle, npm y PyPI sí
funcionan.

## Decisión
- No se rodea el bloqueo con espejos de terceros: se documenta.
- Versiones en `gradle/libs.versions.toml` elegidas por conocimiento previo (AGP 8.11.1, Compose BOM
  2025.06.00, Room 2.7.2, etc.), **sin verificar** contra Google Maven. Las de Maven Central
  (WebRTC, osmdroid, OkHttp, Robolectric, Kotlin) sí se verificaron.
- Verificación de tipos de `:app` con un arnés offline: `android-all` de Robolectric + Compose
  Multiplatform de escritorio + stubs. Compiló (`rc=0`) y encontró un import faltante real.
- Contrato de OpenAI verificado con los tipos del SDK npm `openai` 7.43.0.

## Consecuencias
− El primer `./gradlew :app:assembleDebug` en una máquina con acceso puede requerir ajustar versiones.
− La app no se ha ejecutado. Pruebas en vivo pendientes.
→ Revisar en cuanto haya acceso: ver [[06-TASKS/CONTINUIDAD]].
