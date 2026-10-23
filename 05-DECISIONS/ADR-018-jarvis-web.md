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
- **Qué no hace la web**: notas de Obsidian, cámara, contactos, SMS, alarmas del sistema, habilidades
  de la app. Llamar, rutas, compartir y calendario dejan un botón que la persona toca.

## Consecuencias
+ Una sola URL (`https://<servicio>.onrender.com/`), sin instalar; probada en Chromium real (Playwright)
  en local y en CI, incluida la voz WebRTC contra un par simulado.
− `localStorage` no está cifrado: el token queda expuesto si alguien usa ese navegador o si hubiera XSS
  (mitigado con CSP y DOM seguro). «Borrar todo» en Ajustes lo elimina.
− Avisos de tareas solo con la página abierta (sin service worker ni push).
− Dos implementaciones de herramientas (Kotlin y JS) que deben mantenerse alineadas con el contrato.
