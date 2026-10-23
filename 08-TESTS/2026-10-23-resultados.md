---
tipo: pruebas
fecha: 2026-10-23
entorno: contenedor Linux en la nube, Node 22.22.0, JDK 21, Gradle 8.14.3 (wrapper); sin Android SDK, sin emulador, sin dispositivo
---
# Resultados de pruebas — 2026-10-23

## Ronda 3 — claves del usuario (última)
| Área | Clasificación | Resultado | Comando |
|---|---|---|---|
| Backend (9 archivos, incl. `trade.test.js`, `google-places.test.js`) | **AUTOMATIZADO** | 72/72 ✔ | `cd backend && npm test` |
| Núcleo `:core` (13 clases, incl. `UserApisTest`) | **AUTOMATIZADO** | 143/143 ✔ | `./gradlew -Pjarvis.coreOnly=true :core:test` |
| Kotlin ↔ Node real con Comtrade simulado | **AUTOMATIZADO** | 8/8 ✔ | `BackendIntegrationTest` |
| Tipos de `:app` (mapa sin datos de Google) | **AUTOMATIZADO (parcial)** | `rc=0` | arnés offline |
| UN Comtrade y Google Places reales | **NO TESTEADO** | — | hosts no accesibles desde el entorno; sin las claves aquí |

Datos de prueba sintéticos con la forma de los clientes oficiales (`comtradeapicall` 1.3.2,
`@googlemaps/places` 3.1.0). Verifican la lógica y el contrato, no los servicios reales.

## Ronda 2 — sistema de habilidades
| Área | Clasificación | Resultado | Comando |
|---|---|---|---|
| Backend (7 archivos, incl. `media.test.js`) | **AUTOMATIZADO** | 65/65 ✔ | `cd backend && npm test` |
| Núcleo `:core` (12 clases) | **AUTOMATIZADO** | 138/138 ✔ | `./gradlew -Pjarvis.coreOnly=true :core:test --rerun` |
| Habilidades: contrato común (44) | **AUTOMATIZADO** | 13/13 ✔ | `SkillContractTest` |
| Habilidades: pruebas específicas | **AUTOMATIZADO** | 56/56 ✔ | `SkillsProductivityTest` 16, `SkillsAssistantResearchTest` 18, `SkillsFinanceLifeTest` 22 |
| Integración Kotlin ↔ Node real (visión y transcripción incluidas) | **AUTOMATIZADO** | 7/7 ✔ | `BackendIntegrationTest` |
| Tipos de `:app` con la integración de habilidades | **AUTOMATIZADO (parcial)** | `rc=0` con stubs | arnés offline (ver abajo) |
| `RoomPersistenceTest.skillRecordsSurviveCloseAndReopen` | **NO TESTEADO** | escrita, no ejecutada | requiere Google Maven + SDK |
| Visión/transcripción contra OpenAI real | **NO TESTEADO** | — | `api.openai.com` bloqueado |
| Cámara, micrófono, selector, intents de correo/WhatsApp | **NO TESTEADO** | — | sin emulador ni teléfono |
| `scripts/test-all.sh` | **AUTOMATIZADO** | salida 0 | resume lo anterior |

Detalle por clase de `:core`: BackendIntegrationTest 7 · BrainTest 12 · GlobeTest 6 ·
IntentDetectorTest 11 · LocalToolsTest 6 · MemoryNotesTasksTest 10 · RealtimeTest 9 ·
SkillContractTest 13 · SkillsAssistantResearchTest 18 · SkillsFinanceLifeTest 22 ·
SkillsProductivityTest 16 · ToolsInfraTest 8.

Estado de cada habilidad: [[02-ARCHITECTURE/habilidades|catálogo de habilidades]].

### Errores encontrados y corregidos en esta ronda
- `voice_memo_inbox`: «mañana a las 10» se convertía en tarea a las 00:00 (la hora solo se aceptaba
  tras «el lunes…»). Regex corregida; la prueba lo cubre.
- Notas: una etiqueta escrita como código (`` `#privado` ``) volvía privada la nota, a diferencia de
  Obsidian; `[[nota\|alias]]` en tablas guardaba el enlace con la barra invertida, y los enlaces
  escritos como código contaban como enlaces. Corregidos con prueba en `MemoryNotesTasksTest`
  (y el lint de bóvedas aplica las mismas reglas).
- Recibos sin moneda legible: ahora la confirmación avisa «supuse colones».

### Skills de Claude Code (`JARVIS/.claude/skills/`)
Cada script se ejecutó con un caso correcto y uno con fallos inyectados (AUTOMATIZADO, manual):
| Script | Caso correcto | Caso con fallos |
|---|---|---|
| `architecture-audit/scripts/audit.py` | 16/16 reglas | detecta import Android en `:core`, `GlobalScope`, manifiesto con 2 ejemplos (rc=1) |
| `android-build/scripts/diagnose.sh` | Java 21, wrapper 8.14.3, Maven Central OK | informa SDK ausente y Google Maven bloqueado (rc=1) |
| `realtime-testing/scripts/smoke.sh` | offline ✔; live contra backend local ✔ | token incorrecto → FALLA (rc=1) |
| `api-research/scripts/check-maven-version.sh` | zxing 3.5.4 existe, express 5.3.0 existe | versión inexistente → NO EXISTE; Google Maven → NO VERIFICABLE |
| `obsidian-integration/scripts/vault_lint.py` | KB: 0 enlaces rotos, 0 secretos | bóveda de prueba: clave detectada sin mostrarla, privadas y enlace roto (rc=1) |
| `security-review/scripts/scan-secrets.sh` | 14/14 | clave plantada detectada (rc=1) |
| `business-diagnostics/scripts/business-search.sh` | backend real + OSM simulado: estados correctos | anomalías inyectadas detectadas (rc=1) |
| `session-continuity/scripts/check-kb.py` | KB al día | fecha futura → lista lo que falta (rc=1) |

## Ronda 1

## Resumen
| Área | Clasificación | Resultado | Comando |
|---|---|---|---|
| Backend (6 archivos) | **AUTOMATIZADO** | 57/57 ✔ | `cd backend && npm test` |
| Núcleo `:core` (8 clases) | **AUTOMATIZADO** | 67/67 ✔ | `./gradlew -Pjarvis.coreOnly=true :core:test --rerun` |
| Integración Kotlin ↔ backend Node real | **AUTOMATIZADO** | 5/5 ✔ (incluidas en las 67) | `BackendIntegrationTest` |
| Tipos de `:app` (arnés offline) | **AUTOMATIZADO (parcial)** | compila (`rc=0`) con stubs | ver abajo |
| App Robolectric (`RoomPersistenceTest`, `HomeUiTest`) | **NO TESTEADO** | escritas, no ejecutadas | requiere Google Maven + Android SDK |
| Pruebas en vivo (OpenAI, OSM, búsqueda) | **NO TESTEADO** | — | dominios bloqueados en el entorno |
| Compilación con AGP / APK | **NO TESTEADO** | no hay APK | `dl.google.com` bloqueado |
| Emulador | **NO TESTEADO** | — | no disponible |
| Dispositivo físico | **NO TESTEADO** | — | no disponible |
| Palabra clave Sherpa-ONNX | **NO TESTEADO** | código sin compilar | `github.com` bloqueado |

`scripts/test-all.sh` → salida 0 con este mismo resumen.

## Detalle `:core`
| Clase | Pruebas |
|---|---|
| BrainTest | 12 |
| IntentDetectorTest | 11 |
| MemoryNotesTasksTest | 10 |
| RealtimeTest | 9 |
| ToolsInfraTest | 8 |
| GlobeTest | 6 |
| LocalToolsTest | 6 |
| BackendIntegrationTest | 5 |

## Qué significan (y qué no)
- Las pruebas del backend usan `fetch` simulado: verifican la lógica, la validación, la seguridad y el
  formato de las peticiones a OpenAI/OSM tal como lo definen sus tipos y documentación. **No**
  prueban que los servicios reales respondan así.
- `BackendIntegrationTest` arranca `node src/server.js` de verdad y usa MockWebServer en lugar de
  OpenAI, Overpass, Nominatim y SearXNG.
- Las respuestas OSM de prueba son **sintéticas** con la forma documentada.

## Arnés de tipos de `:app`
Compilación de todo el código de `:app` + `:core` con `android-all` de Robolectric (API 35), Compose
Multiplatform 1.8.2 de escritorio y stubs mínimos para lo que falta. Resultado `rc=0`.
- Bug real encontrado y corregido: faltaba `import androidx.compose.ui.graphics.drawOutline` en
  `ui/components/Hud.kt` y `ui/home/HomeWidgets.kt`.
- Errores restantes eran artefactos del arnés (`SavedState.getString`, `nativeCanvas.drawPoints` de
  Skia) y se resolvieron con shims solo en el arnés.
- Limitación: no valida recursos, manifiesto, KSP/Room ni versiones reales de AndroidX.

## Siguiente ronda (en una máquina con Android Studio)
1. `scripts/build-apk.sh` → corrige lo que falle de versiones.
2. Instala en el teléfono y sigue el plan manual de `JARVIS/tests/README.md`.
3. `cd backend && npm run test:live` con el `.env` real.
4. Anota aquí resultados con modelo de teléfono y versión de Android.

## Ronda: APK en CI y JARVIS Web (2026-10-23, tarde)

| Suite | Resultado | Clasificación | Dónde |
|---|---|---|---|
| Backend `npm test` | 84/84 | **AUTOMATIZADO** | local (Node 22), sin red |
| JARVIS Web `npm run test:e2e` | 9/9 | **AUTOMATIZADO** (Chromium headless 1194, Playwright 1.56.1) | local |
| JARVIS Web en CI | 9/9 + 84/84 del servidor | **AUTOMATIZADO** | GitHub Actions, ejecución 39974699365 (capturas: artefacto `jarvis-web-capturas`) |
| Núcleo `:core:test` | verde | **AUTOMATIZADO** | GitHub Actions, ejecución 39971062825 |
| APK `assembleDebug` | generado (35 MB) | **AUTOMATIZADO** (compilación) | GitHub Actions; Release `jarvis-ultimo` |
| App Robolectric `:app:testDebugUnitTest` | verde (HomeUiTest corregido con reloj pausado) | **AUTOMATIZADO** | GitHub Actions |
| Voz web con OpenAI real | — | **NO TESTEADO** | sin clave ni red hacia OpenAI |
| JARVIS Web desplegada en Render | — | **NO TESTEADO** | falta que el usuario despliegue |

Qué cubren las 9 pruebas de navegador (`backend/test/e2e/web.e2e.js`): token y conexión; HUD, reloj,
clima y globo dibujado; chat con `business_search` de punta a punta (navegador → Express → OpenAI
simulado → herramienta en el navegador → segunda ronda con `function_call_output`); confirmación de
`forget` cancelada sin borrar; texto del modelo sin HTML ni enlaces `javascript:`; tareas con fecha en
español y memoria privada fuera de las instrucciones; clips → «Subir a YouTube» con confirmación y
privacidad `private`; **voz WebRTC real** contra un segundo `RTCPeerConnection` que hace de OpenAI
(SDP, `session.update` con herramientas, herramienta por voz, `end_conversation` y colgado); móvil
390×844 sin desbordes.

Bugs encontrados por estas pruebas y corregidos: el panel lateral tapaba el menú (no se podía cambiar
de panel); el globo se solapaba con el estado y el botón; punto doble tras «p. m.» al crear tareas.

## Ronda: JARVIS Web completa (foto, dictado, instalable, copia, sin conexión)

| Suite | Resultado | Clasificación | Dónde |
|---|---|---|---|
| Backend `npm test` | 85/85 | **AUTOMATIZADO** | local |
| JARVIS Web `npm run test:e2e` | 14/14 | **AUTOMATIZADO** (Chromium 1194) | local |
| JARVIS Web en CI | 14/14 + 85/85; APK y Robolectric también en verde | **AUTOMATIZADO** | GitHub Actions, ejecución 40004045716 |
| Foto con OpenAI real / dictado con OpenAI real | — | **NO TESTEADO** | sin clave ni red |
| Instalar en un teléfono real, avisos de Android | — | **NO TESTEADO** | sin dispositivo |

Nuevas pruebas de navegador: foto de tarjeta (la imagen llega como JPEG reducido; el servidor valida los
bytes y pide salida estricta; los campos vacíos se muestran «no se lee»; «Recordar» guarda en memoria),
dictado (micrófono simulado de Chromium → WebM real → transcripción simulada → texto en el cuadro),
clima desde el menú sin llamar al modelo, copia descargada sin token y restaurada con confirmación,
service worker que controla la página, abre en modo sin conexión y responde la hora en local.
