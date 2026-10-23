---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-016: Integrar UN Comtrade y Google Places; descartar el resto de claves de Google Cloud

## Contexto
El usuario tiene ~28 claves, casi todas de APIs de Google Cloud que se activan por defecto
([[07-RESEARCH/apis-del-usuario|análisis]]). Solo dos aportan algo real a JARVIS. Google Places tiene
costo y condiciones de uso que no pudieron releerse en la página oficial desde el entorno.

## Decisión
- **UN Comtrade** → nueva herramienta `trade_statistics` (servidor `/api/trade/stats`) y la habilidad
  `import_export_research` la usa cuando hay código SA (dado por el usuario o sugerido de una tabla
  corta, avisando que debe confirmarse). Clave en cabecera, caché de 24 h, máximo diario configurable.
- **Google Places (New)** → solo como **segunda fuente al verificar** un negocio de OpenStreetMap
  (`business_verify`). Se acepta un lugar únicamente si el nombre coincide (similitud ≥ 0,6) y está a
  ≤ 200 m; si no, queda como candidato. Nunca se completa un dato sin fuente: el campo lleva `google`.
- Prudencia con las condiciones de Google: los datos con fuente `google` **no se muestran en el mapa
  de OpenStreetMap** y **no se guardan** en el teléfono (prospectos); `Business.withoutRestrictedSources()`
  los quita. En el servidor solo viven en la caché (`BUSINESS_CACHE_TTL_SECONDS`, 1 h por defecto).
  Atribución "Google Maps" en la ficha.
- Ambas desactivadas sin clave (estado visible en `/health`); límites diarios para controlar costos.
- No se integran Calendar (el teléfono ya lee el calendario local), BigQuery, Storage, Logging, etc.

## Consecuencias
+ Estadísticas oficiales en vez de cifras de buscador; más teléfonos y sitios con fuente en Costa Rica.
− Google cuesta dinero pasado el cupo gratuito; con 30/día se queda, en principio, dentro del cupo.
− Si las condiciones vigentes de Google fueran más permisivas o más estrictas, revisar este ADR.
