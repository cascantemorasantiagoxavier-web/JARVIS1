---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-008: Proveedores de búsqueda web en cascada, gratuitos primero

## Contexto
Se necesita búsqueda real con fuentes. La herramienta de búsqueda de OpenAI es de pago por llamada.

## Decisión
- `SEARCH_PROVIDERS=auto`: SearXNG (autoalojado) → Tavily → Brave → OpenAI (solo si
  `SEARCH_ENABLE_OPENAI=true`) → Wikipedia.
- Resultados normalizados (título, URL, fragmento, fecha, proveedor), sin duplicados.
- `fetch_page` lee páginas con protección SSRF; el contenido se marca como no confiable.

## Consecuencias
+ Costo cero posible con SearXNG.
− Calidad depende del proveedor configurado. Ver [[07-RESEARCH/busqueda-web]].
