---
tipo: continuidad
actualizado: 2026-10-23
---
# Nota de continuidad

**Para quien retome el trabajo (persona o agente): empieza aquí.**

## Dónde quedó
Versión 0.1 + **sistema de habilidades**. Backend 65 pruebas y núcleo 138 pruebas, todas en verde
(AUTOMATIZADO, servicios externos simulados). 44 habilidades de la app con estado real en
[[02-ARCHITECTURE/habilidades|el catálogo]] (37 implementadas y 7 parciales con servidor y bóveda).
Visión (`/api/vision/analyze`) y transcripción (`/api/audio/transcribe`) en el servidor. 8 skills de
Claude Code en `JARVIS/.claude/skills/` con scripts probados. La app Android está escrita entera
(incluida la integración de habilidades: Room v2, cámara, selector, grabadora, pantalla *Skills*) pero
**nunca se ha compilado con el Android Gradle Plugin ni ejecutado**: el entorno no tiene Google Maven,
emulador ni teléfono. **No existe APK.**

## Próxima tarea concreta
1. Hecho (2026-10-23): código en `cascantemorasantiagoxavier-web/JARVIS`, rama `jarvis-v0.1-habilidades`
   (`master` es su proyecto anterior, intacto). Esta KB en el repo **privado** `JARVIS1` (rama `main`).
   Cada push a la rama dispara `.github/workflows/build.yml`.
2. Seguir la ejecución con la API pública (ver skill `android-build`): si falla la compilación del APK,
   leer las anotaciones, corregir, commit y push hasta que salga en verde. El APK queda como artefacto
   `jarvis-debug-apk` (el usuario lo descarga desde Actions; no hace falta Android Studio).
3. Desplegar el servidor en Render con `render.yaml` (New → Blueprint); el usuario pone las claves.
4. Pruebas en el teléfono (plan 1–16 de `JARVIS/tests/README.md`); registrar en `08-TESTS/`.

Antes de empezar: `.claude/skills/android-build/scripts/diagnose.sh` dice qué se puede verificar en
esa máquina. Al terminar: skill `session-continuity`.

## Riesgos conocidos
- Cámara/selector/grabadora (`MediaBroker`) y los intents de correo, SMS, WhatsApp y documentos solo
  se verificaron con el arnés de tipos. Si Android mata la app mientras la cámara está abierta, la
  habilidad recibe "cancelado" (aceptable, pero comprobarlo).
- Visión y transcripción probadas solo con OpenAI simulado; confirmar que el modelo de texto
  configurado acepta imágenes (`OPENAI_VISION_MODEL`).
- Versiones de AndroidX sin verificar.
- `voice/wakeword` variante `sherpa`: API escrita sin compilar contra el AAR.
- Contrato Realtime verificado con el SDK de npm, no contra la API real.
- Precio de `gpt-realtime-2.1` (modelo completo) sin confirmar.

## Archivos clave
- `JARVIS/CLAUDE.md` — mapa del código y reglas.
- `JARVIS/docs/SETUP.md` — instalación.
- `JARVIS/shared/tools.json` — herramientas.
- `JARVIS/app/Android/core/.../brain/JarvisBrain.kt` — cerebro.
- `JARVIS/app/Android/core/.../realtime/RealtimeSessionController.kt` — voz.
- `JARVIS/app/Android/app/.../AppContainer.kt` — conexión de todo en Android.

## Esperando al usuario
- **Clave de OpenAI** en `backend/.env` (no la tiene en su lista; sin ella no hay voz, chat, visión).
- Instalar la app de Claude en GitHub (`cascantemorasantiagoxavier-web/JARVIS`): la subida dio 403.
  El código está commiteado en local, con el remoto `origin` ya configurado; rama prevista
  `jarvis-v0.1-habilidades` (no tocar `master`).
- Poner `UN_COMTRADE_API_KEY` y `GOOGLE_MAPS_API_KEY` en el servidor; activar Places API (New) y
  restringir la clave ([[07-RESEARCH/apis-del-usuario]]).
- Servidor en **Render** (confirmado por el usuario; ya tiene allí sus claves, su servicio anterior usaba `OPENAI_API_KEY`). `JARVIS/render.yaml` listo (Blueprint); falta subir el código a GitHub y desplegar.
- Modelo de su teléfono y versión de Android.
