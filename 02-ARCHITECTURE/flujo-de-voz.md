---
tipo: arquitectura
actualizado: 2026-10-23
---
# Flujo de voz Realtime

## Inicio (modo unificado, por defecto)
1. El usuario toca el núcleo o dice "JARVIS" (si activó la palabra clave).
2. `VoiceManager.startConversation` → `RealtimeSessionController.start`.
3. `WebRtcRealtimeConnector` crea la `PeerConnection` (audio con AEC y supresión de ruido por
   hardware), abre el canal `oai-events` y genera la oferta SDP (espera ICE ≤ 2,5 s).
4. `POST /api/realtime/call` al servidor con la oferta. El servidor envía a OpenAI
   `/v1/realtime/calls` (multipart `sdp` + `session`) con su clave y devuelve la respuesta SDP y el
   `callId`.
5. Al abrirse el canal, el teléfono envía `session.update` con instrucciones y las herramientas de
   `shared/tools.json`.

## Conversación
- Turnos con `semantic_vad` e `interrupt_response`: si el usuario habla, OpenAI corta la respuesta y
  el teléfono limpia el audio pendiente (`output_audio_buffer.clear`).
- Herramientas: llega `response.function_call_arguments.done` → mismo `ToolExecutor` que el chat
  (permisos, confirmación) → `conversation.item.create` con `function_call_output`. Cuando todas
  las llamadas de esa respuesta terminan → `response.create`.
- `response.done` trae `usage`: se acumula y se muestra el costo estimado del día.

## Cierre
- Inactividad (30 s por defecto), duración máxima (10 min), botón, notificación "Detener" o la
  herramienta `end_conversation` (cierra cuando termina de sonar la despedida).
- El servidor también cuelga por su cuenta al vencer el tiempo máximo (vigilante de costos).

## Fallos
- Reconexión limitada (`maxReconnects` = 2) con generación por transporte para descartar eventos
  viejos.
- `session_expired` detiene la sesión. Si Realtime no está disponible, se pasa a voz local
  (SpeechRecognizer es-CR + TTS) con aviso.

## Modo "efímero" (alternativo)
El servidor emite un client secret (`/v1/realtime/client_secrets`, TTL 60 s configurado) y el
teléfono negocia directamente con OpenAI. Ver [[05-DECISIONS/ADR-002-realtime-unificado]].
