---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-002: Realtime por WebRTC con negociación "unificada" en el servidor por defecto

## Contexto
La clave permanente de OpenAI no puede llegar al teléfono. OpenAI ofrece dos caminos oficiales para
WebRTC: (a) el servidor crea un *client secret* efímero y el teléfono negocia directamente; (b) el
servidor recibe la oferta SDP y la reenvía a `/v1/realtime/calls` con su clave ("interfaz unificada").

## Decisión
- Por defecto **unificado**: el teléfono envía su SDP a `/api/realtime/call`; el servidor añade la
  configuración de sesión (modelo, voz, VAD, transcripción) y devuelve el SDP de respuesta y el
  `callId`. El teléfono nunca ve credenciales de OpenAI.
- El servidor puede colgar (`/hangup`) y aplica un vigilante de duración máxima.
- **Efímero** queda como opción en Ajustes (TTL configurado 60 s).
- WebRTC en Android: `io.github.webrtc-sdk:android` 144.7559.15 (BSD-3), firmas verificadas con
  `javap`.

## Consecuencias
+ Máxima protección de la clave; control de costos en el servidor.
+ El audio sigue yendo directo teléfono ↔ OpenAI (la negociación es lo único que pasa por el servidor).
− Requiere que el servidor esté disponible para iniciar cada conversación.
