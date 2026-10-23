---
tipo: investigacion
actualizado: 2026-10-23
fuentes:
  - paquete npm "openai" 7.43.0 (definiciones de tipos de realtime)
  - https://developers.openai.com/api/docs/models/gpt-realtime-2.1-mini (vía buscador)
---
# OpenAI Realtime — contrato GA usado por JARVIS

> Cómo se verificó: la documentación web de OpenAI y `api.openai.com` estaban bloqueadas en el
> entorno de desarrollo. Los nombres de rutas, eventos y campos se verificaron leyendo los tipos
> TypeScript del SDK oficial `openai` 7.43.0 (npm). Nada de esto se ha probado contra la API real.

## Rutas
| Ruta | Uso |
|---|---|
| `POST /v1/realtime/client_secrets` | Crea un client secret efímero. `expires_after.seconds` 10–7200 (600 por defecto). Cuerpo con `session` |
| `POST /v1/realtime/calls` | Inicia una llamada WebRTC. Con client secret: cuerpo = SDP. Con clave del servidor: multipart `sdp` + `session` (JSON). Respuesta: SDP; el id de llamada viene en `Location` |
| `POST /v1/realtime/calls/{call_id}/hangup` | Cuelga la llamada |

## Sesión que envía el servidor (`backend/src/services/openai/realtime.js`)
```json
{
  "type": "realtime",
  "model": "gpt-realtime-2.1-mini",
  "instructions": "…persona…",
  "output_modalities": ["audio"],
  "max_output_tokens": 1500,
  "truncation": { "type": "retention_ratio", "retention_ratio": 0.8 },
  "audio": {
    "input": {
      "noise_reduction": { "type": "near_field" },
      "turn_detection": { "type": "semantic_vad", "eagerness": "auto",
                          "create_response": true, "interrupt_response": true },
      "transcription": { "model": "gpt-4o-mini-transcribe", "language": "es" }
    },
    "output": { "voice": "cedar" }
  }
}
```
El teléfono añade instrucciones completas y herramientas con `session.update` al abrir el canal.

## Eventos (canal de datos `oai-events`)
- Cliente → servidor: `session.update`, `response.create`, `response.cancel`,
  `conversation.item.create` (`function_call_output`), `output_audio_buffer.clear`.
- Servidor → cliente usados: `session.created/updated`, `input_audio_buffer.speech_started/stopped`,
  `input_audio_buffer.timeout_triggered`, `response.created`, `response.done` (con `usage`),
  `response.function_call_arguments.done`, `response.output_audio_transcript.delta/done`,
  `conversation.item.input_audio_transcription.completed/failed`,
  `output_audio_buffer.started/stopped/cleared`, `rate_limits.updated`, `error`.

## Modelos y voces
- Modelos: `gpt-realtime-2.1`, `gpt-realtime-2.1-mini` (también `gpt-realtime`, `gpt-realtime-mini`).
- Voces: alloy, ash, ballad, coral, echo, sage, shimmer, verse, **marin**, **cedar**
  (las dos últimas, recomendadas por OpenAI).

## Pendiente de verificar con la API real
- Que `/v1/realtime/calls` acepte exactamente el multipart que arma el servidor.
- Latencia real teléfono ↔ OpenAI desde Costa Rica.
- Comportamiento de `semantic_vad` en español con ruido de calle.
Comando: `cd JARVIS/backend && npm run test:live` y prueba manual en teléfono.

Costos: [[07-RESEARCH/costos]].
