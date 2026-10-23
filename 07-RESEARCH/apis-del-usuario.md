---
tipo: investigacion
actualizado: 2026-10-23
---
# APIs que el usuario tiene (solo nombres, nunca valores)

El 2026-10-23 el usuario compartió los **nombres** de sus variables de claves. Ninguna clave se pegó en
el chat ni se guarda aquí. Valores: solo en `backend/.env` del servidor o en los secretos del hosting.

## Resumen
| Clave | ¿La usa JARVIS? | Para qué | Estado |
|---|---|---|---|
| `UN_COMTRADE_API_KEY` | **Sí** | Estadísticas oficiales de importación/exportación por partida SA (`trade_statistics`, habilidad `import_export_research`) | Integrada; AUTOMATIZADO con datos simulados |
| `GOOGLE_MAPS_API_KEY` | **Sí** | Segunda fuente al verificar un negocio (Places API (New)) | Integrada; AUTOMATIZADO con datos simulados; requiere activar la API y facturación |
| `GOOGLE_CALENDAR_API_KEY` | No | Una clave simple solo lee calendarios **públicos** (según la documentación de Google; no se pudo releer hoy). El teléfono ya lee tu calendario (incluido Google Calendar sincronizado) por Android, sin clave | — |
| `CALENDAR_MCP_API_KEY` | No | No hay servidor MCP de calendario en JARVIS | — |
| `GOOGLE_CLOUD_API_KEY` | No por ahora | Genérica. Si es una clave de Gemini (AI Studio) podría servir de modelo alternativo o búsqueda con Google; no integrado | Decisión pendiente del usuario |
| BigQuery (×7), Cloud Storage (×2), Cloud SQL, Datastore, Dataplex, Dataform, Logging, Monitoring, Trace, Telemetry, App Topology, Analytics Hub, Service Management, Service Usage | No | Son las APIs que Google Cloud **activa por defecto** en un proyecto nuevo; casi todas exigen cuenta de servicio/OAuth, no una clave simple. JARVIS no necesita base de datos en la nube | Recomendación: no crear claves para ellas o borrarlas |

## Lo que falta y es imprescindible
- **`OPENAI_API_KEY`**: no está en la lista. Sin ella no hay voz GPT Realtime, chat con IA, visión ni
  transcripción. Se crea en platform.openai.com (con saldo) y va en `backend/.env`.
- **Búsqueda web**: sin `TAVILY_API_KEY`, `BRAVE_API_KEY` o un SearXNG propio, solo queda Wikipedia
  (pobre para precios, proveedores y noticias). Opción más simple: Tavily (plan gratuito con créditos).

## UN Comtrade — contrato verificado
Fuente: cliente oficial de la ONU `comtradeapicall` 1.3.2 (PyPI, 2026-10-23). Las páginas
`uncomtrade.org` y `comtradedeveloper.un.org` no resolvían desde el entorno.
- `GET https://comtradeapi.un.org/data/v1/get/{C|S}/{A|M}/HS` con `reporterCode`, `partnerCode`,
  `period`, `cmdCode`, `flowCode` (`M` importación, `X` exportación), `maxRecords`, `includeDesc`,
  `breakdownMode`. Registros en `data` con `reporterDesc`, `partnerCode`, `partnerDesc`, `cmdCode`,
  `cmdDesc`, `flowCode`, `flowDesc`, `period`, `primaryValue` (USD).
- La clave: el cliente oficial la manda como parámetro `subscription-key`; el paquete `comtradr`
  (rOpenSci) como cabecera `Ocp-Apim-Subscription-Key`. JARVIS usa la cabecera para que no quede en logs.
- **Sin verificar**: cuota exacta del plan gratuito (fuentes de terceros dicen 500 llamadas/día y hasta
  100 000 registros por llamada) y si `partnerCode=0` es siempre "World" (se detecta también por
  `partnerDesc`). Costa Rica = 188 (código M49; conocimiento general, no releído hoy).
- JARVIS limita a 200 consultas/día (`COMTRADE_MAX_REQUESTS_PER_DAY`) y guarda resultados 24 h en caché.

## Google Places API (New) — contrato verificado
Fuente: cliente oficial `@googlemaps/places` 3.1.0 (npm) y sus `.proto`. `developers.google.com` no
resolvía desde el entorno.
- `POST https://places.googleapis.com/v1/places:searchText`; cabecera `X-Goog-FieldMask` obligatoria;
  cuerpo `textQuery`, `languageCode`, `regionCode`, `maxResultCount` (1–20),
  `locationBias.circle {center {latitude, longitude}, radius ≤ 50000}`.
- Campos usados: `id`, `displayName.text`, `formattedAddress`, `location`, `nationalPhoneNumber`,
  `internationalPhoneNumber`, `websiteUri`, `googleMapsUri`, `businessStatus`.
- **Precio (sin verificar en la página oficial)**: según los resultados del buscador sobre la página de
  precios de Google, "Text Search Enterprise" tiene 1 000 solicitudes gratis al mes y luego unos US$35
  por 1 000. Pedir teléfono o sitio web probablemente cae en ese nivel. JARVIS limita a 30 por día.
- **Condiciones (sin verificar en la página oficial)**: según versiones archivadas de las condiciones,
  el contenido de Places no se usa junto a un mapa que no sea de Google, requiere atribución si se
  muestra sin mapa y las coordenadas solo se guardan 30 días. Por prudencia: [[05-DECISIONS/ADR-016-apis-del-usuario|ADR-016]].

## Pasos para el usuario
1. Crear la clave de OpenAI y ponerla en `backend/.env`.
2. Copiar `UN_COMTRADE_API_KEY` y `GOOGLE_MAPS_API_KEY` en `backend/.env` (o en los secretos del hosting).
3. En Google Cloud: activar **Places API (New)**, tener facturación activa y **restringir la clave**
   a esa API (y a la IP del servidor si es fija).
4. Borrar o no usar las claves de APIs que JARVIS no necesita: menos claves, menos riesgo.
