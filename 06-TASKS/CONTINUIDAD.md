---
tipo: continuidad
actualizado: 2026-10-24
---
# Nota de continuidad

**Para quien retome el trabajo (persona o agente): empieza aquí.**

## Dónde quedó
Versión 0.1 + habilidades + **JARVIS Web**. Código en `cascantemorasantiagoxavier-web/JARVIS`, rama
`jarvis-v0.1-habilidades` (`master` es su proyecto anterior, intacto); esta KB en el repo privado
`JARVIS1` (`main`). GitHub Actions en verde: núcleo, **APK compilado con AGP** (publicado en la Release
`jarvis-ultimo`) y pruebas Robolectric de la app. Backend 85 pruebas + 14 de navegador (Chromium real): chat, voz, foto, dictado, paneles, copia de seguridad, instalable y sin conexión.
El usuario prefiere la **web**: el servidor sirve el HUD en `/` ([[05-DECISIONS/ADR-018-jarvis-web|ADR-018]]).
**Cerebro: Gemini** (no OpenAI), Llama de respaldo ([[05-DECISIONS/ADR-019-gemini-cerebro|ADR-019]]); voz de la web con Gemini Live.
Nada se ha probado todavía contra OpenAI ni OpusClip reales, ni en un teléfono.

## Próxima tarea concreta
1. Que el usuario despliegue en Render (New → Blueprint con `render.yaml`) y ponga sus claves en
   Environment. Después abrir `https://<servicio>.onrender.com/`, pegar `JARVIS_CLIENT_TOKEN` y seguir
   la lista manual «JARVIS Web en un navegador real» de `JARVIS/tests/README.md`; registrar en `08-TESTS/`.
2. Si algo falla en Render: `/health` dice qué está activo; los logs de Render no muestran claves.
3. Clips: plan de OpusClip con API y YouTube conectado en OpusClip; la primera prueba real debe
   confirmar los campos de la API ([[07-RESEARCH/opusclip-youtube]]).
4. App Android (opcional ahora): instalar el APK de la Release y seguir el plan 1–16 de `tests/README.md`.

Antes de empezar: `.claude/skills/android-build/scripts/diagnose.sh` dice qué se puede verificar en
esa máquina. Al terminar: skill `session-continuity`.

## Riesgos conocidos
- Cámara/selector/grabadora (`MediaBroker`) y los intents de correo, SMS, WhatsApp y documentos solo
  se verificaron con el arnés de tipos. Si Android mata la app mientras la cámara está abierta, la
  habilidad recibe "cancelado" (aceptable, pero comprobarlo).
- Visión y transcripción probadas solo con OpenAI simulado; confirmar que el modelo de texto
  configurado acepta imágenes (`OPENAI_VISION_MODEL`).
- Versiones de AndroidX: resueltas y compiladas en GitHub Actions (no en este entorno).
- JARVIS Web: token en `localStorage` (no cifrado); avisos de tareas solo con la página abierta.
- `voice/wakeword` variante `sherpa`: API escrita sin compilar contra el AAR.
- Contrato Realtime verificado con el SDK de npm, no contra la API real.
- Precio de `gpt-realtime-2.1` (modelo completo) sin confirmar.

## Archivos clave
- `JARVIS/CLAUDE.md` — mapa del código y reglas.
- `JARVIS/docs/SETUP.md` — instalación (incluye «Usar JARVIS Web»).
- `JARVIS/backend/public/js/` — JARVIS Web (`main.js`, `brain.js`, `tools.js`, `realtime.js`).
- `JARVIS/shared/tools.json` — herramientas.
- `JARVIS/app/Android/core/.../brain/JarvisBrain.kt` — cerebro.
- `JARVIS/app/Android/core/.../realtime/RealtimeSessionController.kt` — voz.
- `JARVIS/app/Android/app/.../AppContainer.kt` — conexión de todo en Android.

## Despliegue
Servicio en Render: `https://jarvis-9zmd.onrender.com` (URL solo en esta KB privada, no en el repositorio
público). `/health` OK el 2026-10-24. Falta `EXA_API_KEY` en Render. El usuario pegó por error el Deploy
Hook en el chat: se le pidió regenerarlo.

## Esperando al usuario
- Crear el servicio en Render (Blueprint o «Public Git Repository», rama `jarvis-v0.1-habilidades`, raíz
  `backend`) y poner `GEMINI_API_KEY`, `OSM_CONTACT` y `JARVIS_CLIENT_TOKEN`; opcionales `LLAMA_API_KEY`,
  `UN_COMTRADE_API_KEY`, `GOOGLE_MAPS_API_KEY`, `EXA_API_KEY` (eligió Exa para buscar). Nunca por chat.
- Decirnos la URL pública (solo la URL) para comprobar `/health` (`features.ai`, `features.voice`).
- Primera prueba real con Gemini: chat con herramientas, foto, dictado y voz en vivo; registrar en `08-TESTS/`.
- Pendientes de su lista: YouTube Data API v3 (búsqueda de videos), Firebase (sincronizar memoria), Spotify.
