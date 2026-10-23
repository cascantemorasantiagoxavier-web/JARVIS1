---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-003: `gpt-realtime-2.1-mini` y voz `cedar` por defecto

## Contexto
El costo de audio domina el gasto. Según la página del modelo en developers.openai.com (consultada
vía buscador el 2026-10-23), el mini cuesta US$10 / 1M tokens de audio de entrada y US$20 / 1M de
salida. Para `gpt-realtime-2.1` una sola fuente secundaria indica US$32 / US$64 (sin confirmar).
OpenAI recomienda las voces `marin` y `cedar` por su calidad.

## Decisión
- Modelo por defecto `gpt-realtime-2.1-mini`; el usuario puede elegir `gpt-realtime-2.1` en Ajustes
  (lista blanca en `OPENAI_REALTIME_ALLOWED_MODELS`).
- Voz por defecto `cedar` (alternativa `marin`). La voz se fija al crear la sesión.
- Chat de texto: `gpt-5.4-mini` con esfuerzo de razonamiento `low` (DEEP lo sube).

## Consecuencias
+ Costo aproximadamente 3 veces menor en audio si se confirma el precio del modelo completo.
− Algo menos de capacidad de razonamiento en voz; se compensa con herramientas.
