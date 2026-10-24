---
tipo: investigacion
actualizado: 2026-10-23
---
# Proveedores de búsqueda web

Implementados en `JARVIS/backend/src/services/search/providers.js`. Orden con `SEARCH_PROVIDERS=auto`:

| Proveedor | Configuración | Costo | Notas |
|---|---|---|---|
| SearXNG | `SEARXNG_URL` | Gratis (autoalojado) | Activar el formato `json` en `settings.yml`. Agrega varios buscadores |
| Tavily | `TAVILY_API_KEY` | Plan gratuito con créditos mensuales (ver condiciones vigentes) | Pensado para agentes de IA |
| Brave Search API | `BRAVE_API_KEY` | Según condiciones vigentes | Índice propio |
| OpenAI web search | `SEARCH_ENABLE_OPENAI=true` | De pago por llamada | Desactivado por defecto |
| Wikipedia | `WIKIPEDIA_LANG=es` | Gratis | Solo enciclopédico; siempre disponible como último recurso |

## Garantías
- Cada resultado: título, URL, fragmento, fecha si existe, proveedor.
- Deduplicación por URL normalizada.
- El contenido de las páginas es **no confiable**: el modelo recibe la instrucción de no seguir
  órdenes que aparezcan en él.
- `fetch_page` bloquea IP privadas y redirecciones a redes internas (SSRF) y limita el tamaño.

## Estado
Probado con respuestas simuladas (AUTOMATIZADO). Con proveedores reales: NO TESTEADO (bloqueados
en el entorno de desarrollo). Recomendación: configurar SearXNG o Tavily primero.

## Exa (añadido 2026-10-24)
- `POST https://api.exa.ai/search`, clave en la cabecera `x-api-key` (SDK oficial `exa-js` 2.26.0).
- JARVIS envía `type: "auto"`, `numResults`, `includeDomains`, `userLocation: "CR"` y pide extractos con
  `contents.highlights` (600 caracteres). Resultados: `title`, `url`, `publishedDate`, `highlights[]`.
- Orden en `SEARCH_PROVIDERS=auto`: searxng, tavily, **exa**, brave, openai, wikipedia.
- Créditos gratis al registrarse (condiciones de Exa; verificarlas en su página antes de presupuestar).
