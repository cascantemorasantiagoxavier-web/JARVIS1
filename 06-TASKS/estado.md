---
tipo: tareas
actualizado: 2026-10-23
---
# Estado de las tareas

## Hecho
- [x] Estructura `JARVIS/` y `JARVIS-KB/`, `CLAUDE.md`, `AGENTS.md`, reglas `.claude/rules/*`.
- [x] Backend: Realtime (unificado y efímero), chat con herramientas, búsqueda web, negocios OSM,
      límites de costo, seguridad.
- [x] `:core`: cerebro, intenciones en español, 33 herramientas, controlador Realtime, memoria,
      notas, tareas, prospectos, clima, globo.
- [x] `:app`: UI HUD completa (inicio, chat, web, negocios, mapa, tareas, memoria, ajustes), Room,
      DataStore, Keystore, WebRTC, voz local, servicio en primer plano, recordatorios, indexador
      de Obsidian, palabra clave (variantes con y sin Sherpa).
- [x] Verificación de tipos de `:app` con arnés offline.
- [x] Documentación: `README.md`, `docs/SETUP.md`, `docs/HOSTING.md`, `tests/README.md`.
- [x] Scripts: `test-all.sh`, `build-apk.sh`, `fetch-sherpa-kws.sh`.
- [x] Sistema de habilidades: registro declarativo, motor, riesgo y confirmaciones, 44 habilidades
      con pruebas (contrato común + específicas). [[02-ARCHITECTURE/habilidades|Catálogo]].
- [x] Backend: visión y transcripción (65 pruebas en total).
- [x] App: integración de habilidades (Room v2, cámara, selector, grabadora, pantalla *Skills*),
      verificada con el arnés de tipos.
- [x] 8 skills de Claude Code con scripts probados; `docs/SKILLS.md`; ADR-013, 014 y 015.
- [x] Clips de video con OpusClip y publicación en YouTube vía OpusClip (habilidad `video_clips`,
      PARCIAL hasta la primera prueba real). ADR-017.
- [x] Claves del usuario analizadas; integradas UN Comtrade (`trade_statistics`) y Google Places
      (verificación de negocios, datos restringidos). ADR-016.

## Pendiente (requiere al usuario o un entorno sin bloqueos)
- [ ] Compilar con AGP y generar el APK: listo el flujo de GitHub Actions (`.github/workflows/build.yml`,
      validado con actionlint); se ejecutará con el primer push. Alternativa: `scripts/build-apk.sh`.
- [ ] Ejecutar pruebas Robolectric de `:app`.
- [ ] Pruebas en vivo del backend con claves reales (`npm run test:live`).
- [ ] Pruebas manuales en teléfono (plan en `JARVIS/tests/README.md`).
- [ ] Descargar y verificar Sherpa-ONNX (`scripts/fetch-sherpa-kws.sh`).
- [ ] Probar en el teléfono las habilidades con cámara, micrófono, selector e intents (plan 11–16 de
      `JARVIS/tests/README.md`).
- [ ] Partes declaradas como pendientes en las 7 habilidades PARCIALES (PDF, bandeja de correo por
      OAuth, factura electrónica de Hacienda, publicación en redes, disparadores automáticos de
      rutinas, publicación de sitios). Requieren decisión y credenciales del usuario.
- [ ] Desplegar el servidor con HTTPS (`docs/HOSTING.md`).
- [ ] El usuario: indicar los **nombres** de los servicios de sus claves (sin pegarlas).
- [ ] El usuario: dar un repositorio de GitHub para publicar el código.

## Ideas para después
- Escritura opcional en la bóveda de Obsidian (con confirmación).
- Firma del APK release y canal de actualizaciones.
- Persistir los contadores de uso del servidor (hoy en memoria).
