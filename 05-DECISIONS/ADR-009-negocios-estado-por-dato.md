---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-009: Negocios desde OpenStreetMap con estado y fuente por dato

## Contexto
La búsqueda de negocios no puede inventar teléfonos, correos ni direcciones.

## Decisión
- Overpass para buscar por categoría dentro de un área administrativa de Costa Rica (provincia
  `admin_level` 4, cantón 6, distrito 8); Nominatim para resolver zonas (máx. 1 petición/s).
- Cada campo: `value`, `status` (`found`, `verified`, `unavailable`, `conflict`, `approximate`),
  `sources`, `alternatives`.
- `business_verify` contrasta con el sitio web del negocio; sin sitio, devuelve candidatos web
  marcados como NO verificados.
- Prospectos guardados con instantánea de la ficha y fecha de consulta.

## Consecuencias
+ Honestidad verificable en la interfaz (etiquetas por dato).
− La cobertura de OSM en Costa Rica es desigual: muchos negocios sin teléfono.
