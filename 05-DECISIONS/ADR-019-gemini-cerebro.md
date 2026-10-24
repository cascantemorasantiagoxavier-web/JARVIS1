---
tipo: adr
estado: Aceptada
fecha: 2026-10-24
---
# ADR-019: Gemini como cerebro, Llama de respaldo y proveedores intercambiables

## Contexto
El usuario no usará OpenAI: quiere Gemini (Google AI Studio, «ya tienes conexión») y Llama. Todo el
servidor hablaba solo con OpenAI (API Responses, Realtime, visión, transcripción).

## Decisión
- **Capa de proveedores en el servidor** (`backend/src/services/ai`): `gemini.js` (API nativa
  `generateContent`), `compat.js` (Llama por cualquier API compatible con Chat Completions: Groq por
  defecto, Meta Llama API, OpenRouter…) y el código de OpenAI que ya existía. `AI_PROVIDERS=auto` usa
  Gemini → OpenAI → Llama, los que tengan clave; si uno falla por cuota, caída o tiempo, responde el siguiente.
- **Mismo formato hacia la web y la app**: `/api/chat` sigue devolviendo `{text, toolCalls, outputItems}`.
  El contenido original de Gemini (con `thoughtSignature`) viaja dentro de `outputItems[].provider_data`
  y se reenvía intacto en la ronda siguiente; OpenAI recibe los items sin esos datos. Ni la web ni la app
  Android tuvieron que cambiar para el chat, las fotos ni el dictado.
- **Esquemas de herramientas**: se pasan como `parametersJsonSchema` quitando las palabras que la API no
  documenta (`additionalProperties`, `pattern`, `minLength`…). La validación de argumentos sigue en el cliente.
- **Fotos**: Gemini con `responseJsonSchema` (cadenas vacías → `null`); Llama con `json_object` y un aviso
  en las instrucciones. **Dictado**: Gemini entiende el audio directamente; la web graba WAV 16 kHz
  cuando el dictado es de Gemini (Chrome graba WebM, que la API de Gemini no lista). Llama usa Whisper.
- **Voz en vivo con Gemini Live** solo en la web: token efímero de un solo uso con la configuración
  bloqueada (ver `.claude/rules/realtime-voice.md`). El modelo se elige solo (`GEMINI_LIVE_MODEL=auto`).
- **Modelos por defecto**: `gemini-flash-latest` (alias que Google mantiene apuntando al Flash más nuevo)
  para no quedar atados a un nombre que caduque; Llama `auto` elige de la lista del proveedor.

## Consecuencias
+ Un solo cambio de clave en Render; la app y la web siguen igual. Respaldo automático ante cuotas.
+ La clave de Gemini nunca llega al navegador (cabecera `x-goog-api-key` en el servidor; token efímero para la voz).
− Contrato tomado del SDK oficial, sin probar con la clave real del usuario: la primera prueba en Render debe
  confirmar chat, fotos, dictado y voz. Única verificación real: el WebSocket de Gemini Live de Google
  respondió a la URL de la web pidiendo un `access_token` válido (la ruta y el parámetro son correctos).
− La voz en vivo de la app Android sigue siendo OpenAI Realtime; sin esa clave usa la voz local del teléfono.
− `gemini-flash-latest` puede cambiar de modelo sin aviso; si algo se rompe, fijar `GEMINI_MODEL`.
