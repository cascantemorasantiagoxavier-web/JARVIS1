---
tipo: investigacion
actualizado: 2026-10-24
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
| `GEMINI_API_KEY` (Google AI Studio) | **Sí: el cerebro** | Chat con herramientas, fotos, dictado y voz en vivo (Gemini Live) ([[05-DECISIONS/ADR-019-gemini-cerebro|ADR-019]]). El usuario dijo «ya tienes conexión» con AI Studio | Integrada; AUTOMATIZADO con Gemini simulado; nunca con su clave real |
| `LLAMA_API_KEY` | Opcional | Respaldo si Gemini se queda sin cuota (Groq por defecto) | Integrada; AUTOMATIZADO con datos simulados |
| `GOOGLE_CLOUD_API_KEY` | No | Genérica. Si en realidad es de AI Studio, va como `GEMINI_API_KEY` | — |
| BigQuery (×7), Cloud Storage (×2), Cloud SQL, Datastore, Dataplex, Dataform, Logging, Monitoring, Trace, Telemetry, App Topology, Analytics Hub, Service Management, Service Usage | No | Son las APIs que Google Cloud **activa por defecto** en un proyecto nuevo; casi todas exigen cuenta de servicio/OAuth, no una clave simple. JARVIS no necesita base de datos en la nube | Recomendación: no crear claves para ellas o borrarlas |

## Lista del usuario del 2026-10-24 (pegada en el chat, sin claves)
Gemini (cerebro), Places API (New), Maps SDK for Android, Google Calendar API, YouTube Data API v3;
además Brave, Tavily, Open-Meteo, OSM/Overpass, UN Comtrade, Firebase y Spotify.
| Servicio | Estado en JARVIS |
|---|---|
| Gemini | **Cerebro** (ADR-019) |
| Places API (New) | Ya integrada para **verificar** negocios (ADR-016); la búsqueda principal sigue en OSM |
| Maps SDK for Android | No: la app usa osmdroid (OSM, gratis). Cambiarlo exige clave restringida a la app |
| Google Calendar API | No: exige OAuth del usuario; la app lee el calendario del teléfono y la web abre Google Calendar |
| YouTube Data API v3 | Pendiente: herramienta de búsqueda de videos (clave simple, cuota diaria) |
| Brave / Tavily | Ya soportadas (`BRAVE_API_KEY`, `TAVILY_API_KEY`) |
| Open-Meteo, OSM/Overpass, UN Comtrade | Ya integradas |
| Firebase, Spotify | Pendientes; Firebase serviría para sincronizar la memoria entre equipos |

## Lo que falta y es imprescindible
- **`GEMINI_API_KEY`** en Render (el usuario no usará OpenAI). Sin ella no hay chat con IA, fotos,
  dictado ni voz en vivo.
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
