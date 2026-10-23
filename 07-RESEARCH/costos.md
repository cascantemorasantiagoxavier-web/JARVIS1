---
tipo: investigacion
actualizado: 2026-10-23
---
# Costos de OpenAI

> **Verifica siempre en la página oficial de precios antes de presupuestar.** Desde el entorno de
> desarrollo no se pudo abrir la página de OpenAI; estas cifras vienen de resultados de búsqueda
> del 2026-10-23.

## Realtime (USD por 1 millón de tokens)

| Modelo | Audio entrada | Audio entrada en caché | Audio salida | Texto entrada | Texto salida | Confianza |
|---|---|---|---|---|---|---|
| `gpt-realtime-2.1-mini` | 10,00 | 0,30 | 20,00 | 0,60 | 2,40 | Alta: página del modelo en developers.openai.com (vía buscador); coincide con cloudprice.net |
| `gpt-realtime-2.1` | 32,00 | 0,40 | 64,00 | 4,00 | 24,00 | **Baja**: audio de un solo blog (forasoft.com); texto de agregadores |

Estas cifras están en `PriceTable` (`core/.../realtime/RealtimeUsage.kt`) para estimar el costo
del día en Ajustes. La app suma los tokens reales que OpenAI informa en cada `response.done`.

### Ejemplo de cálculo (solo aritmética)
Una conversación que consume 10 000 tokens de audio de entrada y 5 000 de salida con el mini:
10 000 × 10 / 1 000 000 + 5 000 × 20 / 1 000 000 = 0,10 + 0,10 = **US$ 0,20**.
Mide tus conversaciones reales en Ajustes para conocer el consumo por minuto.

## Chat de texto
| Modelo | Entrada | Entrada en caché | Salida | Fuente |
|---|---|---|---|---|
| `gpt-5.4-mini` | 0,75 | 0,075 | 4,50 | Página del modelo en developers.openai.com (vía buscador) |

## Otros costos
- Transcripción de la voz del usuario (`gpt-4o-mini-transcribe`): tiene costo aparte, sin precio
  verificado aquí. Se desactiva con `OPENAI_REALTIME_TRANSCRIPTION_MODEL=none`.
- Búsqueda web de OpenAI: de pago por llamada; desactivada por defecto.
- Tavily y Brave: planes según sus condiciones vigentes. SearXNG autoalojado: gratis.
- OpenStreetMap, Open-Meteo, Natural Earth: gratuitos con sus políticas de uso.

## Controles de costo implementados
- Modelo mini por defecto; ruta local sin IA para intenciones claras.
- Servidor: `REALTIME_MAX_SESSIONS_PER_HOUR=20`, `REALTIME_MAX_SESSIONS_PER_DAY=60`,
  `CHAT_MAX_REQUESTS_PER_DAY=500`, vigilante que cuelga al vencer la duración máxima.
- App: cierre por 30 s de inactividad, máximo 10 min por sesión, contador del día visible.
- Recomendado: límite de gasto mensual en la cuenta de OpenAI.

## Fuentes
- https://developers.openai.com/api/docs/models/gpt-realtime-2.1-mini
- https://cloudprice.net/models/gpt-realtime-2.1-mini
- https://www.forasoft.com/blog/article/openai-realtime-api-pricing
- https://developers.openai.com/api/docs/models/gpt-5.4-mini
