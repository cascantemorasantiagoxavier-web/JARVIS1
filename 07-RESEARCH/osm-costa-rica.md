---
tipo: investigacion
actualizado: 2026-10-23
---
# OpenStreetMap para negocios de Costa Rica

## Fuentes
- **Overpass API** (`OVERPASS_URL`): consulta de puntos de interés por etiquetas dentro de un área.
- **Nominatim** (`NOMINATIM_URL`): resolver nombres de lugares a áreas administrativas.
- Licencia de los datos: **ODbL**, "© colaboradores de OpenStreetMap" (se muestra en cada fuente y
  en el mapa).

## Áreas administrativas usadas
| Nivel | `admin_level` | Ejemplo |
|---|---|---|
| País | 2 | Costa Rica |
| Provincia | 4 | San José, Heredia… |
| Cantón | 6 | Acosta, Escazú… |
| Distrito | 8 | San Ignacio… |

Consulta tipo: área por id de relación (`area(id:3600000000 + rel)`), búsqueda por etiquetas de la
categoría, y `foreach` + `is_in` para obtener provincia/cantón/distrito de cada resultado.

## Políticas de uso que el backend respeta
- Nominatim: máximo 1 petición por segundo, User-Agent identificable con contacto (`OSM_CONTACT`),
  caché de resultados.
- Overpass: consultas acotadas (límite de resultados, timeout) y caché (`BUSINESS_CACHE_TTL_SECONDS`).

## Calidad de datos
- Muchos negocios en OSM no tienen teléfono, correo ni horario: JARVIS muestra "no disponible".
- Varios nodos pueden ser el mismo negocio: se fusionan y se listan los ids fusionados.
- Teléfonos se normalizan a +506 cuando es posible.

## Estado
Lógica probada con respuestas sintéticas con la forma documentada (AUTOMATIZADO). Contra los
servidores reales: NO TESTEADO (bloqueados en el entorno de desarrollo; preparado en
`backend/test/live`).
