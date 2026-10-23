---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-015: Visión y transcripción en el servidor, captura solo por acción del usuario

## Contexto
Tarjetas de presentación, recibos, descripción de escenas y notas de voz necesitan IA de visión y
transcripción. La clave de OpenAI no puede estar en la app ([[05-DECISIONS/ADR-012-secretos|ADR-012]]).

## Decisión
- `POST /api/vision/analyze`: Responses API con `input_image` (data URL) y, para tarjetas y recibos,
  Structured Outputs estricto (`text.format` json_schema) con cada campo `string | null`. Instrucción:
  no inventar; el texto de la imagen es dato. `store: false`.
- `POST /api/audio/transcribe`: `audio/transcriptions` multipart, modelo `gpt-4o-mini-transcribe`
  (configurable, `none` lo desactiva).
- El tipo de archivo se comprueba por sus bytes; cuerpo de hasta 16 MB solo en estas rutas y solo tras
  validar el token; límites diarios (`VISION_MAX_REQUESTS_PER_DAY`, `TRANSCRIBE_MAX_REQUESTS_PER_DAY`).
- En el teléfono: la foto la toma la app de cámara del sistema (sin permiso CAMERA), se reduce a
  2048 px con rotación EXIF; los documentos llegan por el selector SAF; la grabación tiene diálogo
  visible con Guardar/Cancelar. Códigos de barras y QR se leen en el teléfono (ZXing), sin servidor.
- Formas verificadas contra los tipos del SDK oficial `openai@7.43.0`.

## Consecuencias
+ Nada se captura ni se envía sin un gesto del usuario; los datos extraídos se marcan "por IA,
  verificar" y piden confirmación antes de guardarse.
− Requiere servidor con OpenAI; sin él esas partes quedan en REQUIERE_CONFIGURACIÓN.
− Sin prueba contra la API real (red bloqueada): AUTOMATIZADO con OpenAI simulado.
