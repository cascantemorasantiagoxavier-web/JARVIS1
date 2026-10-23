---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-018: JARVIS Web servida por el mismo servidor

## Contexto
El usuario pidió JARVIS «en una página web en vez de APK». La app Android sigue existiendo (APK en la
Release `jarvis-ultimo`), pero una web evita instalar, sirve en computadora y teléfono, y se despliega
junto con el servidor en Render.

## Decisión
- El backend Express sirve `backend/public` en `/` (mismo origen que `/api/*`, sin CORS). Módulos ES
  sin compilación ni dependencias de terceros en el navegador.
- **CSP estricta** (helmet): `script-src 'self'`, sin scripts en línea; `connect-src` solo el propio
  servidor y Open-Meteo. Todo el DOM se construye con `textContent`; enlaces solo `http(s)`/`tel:`.
  Una prueba falla si aparece `innerHTML`, `eval` o algo con forma de clave en `public/`.
- **Mismo contrato y misma política de riesgo**: `public/shared/tools.json` es copia de `shared/`
  (prueba de sincronía). `public/js/tools.js` implementa 28 herramientas con la regla de
  [[05-DECISIONS/ADR-014-riesgo-y-confirmacion|ADR-014]]: sensible o `confirm` → confirmación siempre;
  externa → confirmación salvo toque directo.
- **Cerebro**: persona + contexto (recuerdos NO privados, tareas) + bucle de herramientas con
  `/api/chat`. Sin IA configurada responde en local hora, cálculos y clima.
- **Voz**: GPT Realtime por WebRTC en modo unificado (`/api/realtime/call`); el navegador nunca ve la
  clave. Solo arranca con el botón «Hablar»; inactividad 30 s y máximo 10 min; `end_conversation` cuelga.
- **Datos locales**: memoria, tareas, conversación y proyectos de clips en `localStorage` del navegador.
  El token del servidor también (equivalente al Keystore de Android). Los datos de Google Maps se quitan
  antes de guardar ([[05-DECISIONS/ADR-016-apis-del-usuario|ADR-016]]).
- **Foto y dictado** (añadidos el mismo día): la cámara o una imagen elegida se reduce a JPEG en el
  navegador y va a `/api/vision/analyze` (tarjeta, recibo, documento o pregunta libre); el dictado graba
  con `MediaRecorder` y va a `/api/audio/transcribe`, y el texto queda en el cuadro para revisarlo. Solo
  por toque; nada se guarda salvo una miniatura en la conversación. Lo leído de una foto entra al
  historial marcado como dato, no como instrucción.
- **Instalable y sin conexión**: `sw.js` primero la red y la copia local solo sin conexión; nunca
  intercepta `/api` ni `/health`. Iconos PNG generados desde los SVG (`npm run build-icons`). En Android
  los avisos se muestran con `registration.showNotification`.
- **Copia de seguridad**: exportar/importar un JSON (sin el token) para pasar memoria y tareas a otro equipo.
- **Qué no hace la web**: notas de Obsidian, contactos, SMS, alarmas del sistema, habilidades de la app.
  Llamar, rutas, compartir y calendario dejan un botón que la persona toca.

## Consecuencias
+ Una sola URL (`https://<servicio>.onrender.com/`), sin instalar; probada en Chromium real (Playwright)
  en local y en CI, incluida la voz WebRTC contra un par simulado.
− `localStorage` no está cifrado: el token queda expuesto si alguien usa ese navegador o si hubiera XSS
  (mitigado con CSP y DOM seguro). «Borrar todo» en Ajustes lo elimina.
− Avisos de tareas solo con la página o la app instalada abierta (sin notificaciones push del servidor).
− Dos implementaciones de herramientas (Kotlin y JS) que deben mantenerse alineadas con el contrato.
