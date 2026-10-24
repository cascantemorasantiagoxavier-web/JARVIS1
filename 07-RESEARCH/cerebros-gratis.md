---
tipo: investigacion
actualizado: 2026-10-24
---
# Cerebros (modelos de IA) gratis y rápidos — octubre 2026

Fuentes: blogs agregadores de julio–octubre 2026 (no las páginas oficiales); los límites cambian a
menudo: verificarlos en el panel de cada proveedor.

| Proveedor | Gratis | Límites reportados | Herramientas | En JARVIS |
|---|---|---|---|---|
| **Groq** | Sí, sin tarjeta | ~30 peticiones/min, ~1.000/día; tokens por minuto limitados | Sí (compatible OpenAI) | `GROQ_API_KEY`; modelo `auto` elige gpt-oss-120b / Kimi K2 / Llama |
| **Gemini** (AI Studio) | Sí | Varía mucho: de ~1.500/día a 20–100/día tras recortes de abril 2026 (según fuente); Pro ya no es gratis | Sí | Cerebro, fotos, dictado y voz en vivo |
| OpenRouter (`:free`) | Sí | 20/min y 50/día (1.000/día tras comprar 10 US$) | Según el modelo | `LLAMA_BASE_URL=https://openrouter.ai/api/v1` |
| Cerebras | Dudoso | Algunas fuentes: ya no hay plan gratis permanente (créditos de prueba con tarjeta) | Sí | `LLAMA_BASE_URL` |
| Mistral (Experiment) | Sí | ~1 petición/s; **puede usar tus datos para entrenar** | Sí | No recomendado |
| GitHub Models | Sí | 15/min, 150/día (nivel bajo) | Según el modelo | No integrado |
| SambaNova | Sí | ~20/día reportado | Según el modelo | No recomendado |

Recomendación dada al usuario: **Groq primero y Gemini de respaldo** (`AI_PROVIDERS=groq,gemini`);
Gemini sigue para la voz en vivo y las fotos.
