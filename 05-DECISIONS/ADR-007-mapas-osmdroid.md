---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-007: Mapa con osmdroid y solo bajo petición

## Contexto
El usuario no quiere el mapa como pantalla principal. Google Maps SDK requiere clave y Google Maven.

## Decisión
- osmdroid 6.1.20 (Apache-2.0, Maven Central) con teselas OSM, colores invertidos para el tema
  oscuro y atribución visible.
- Solo coordenadas reales de la fuente; nada aproximado se dibuja como exacto.
- Se abre únicamente con `show_on_map` o un botón. Rutas y navegación: se delega en la app de mapas
  instalada (`get_directions`).

## Consecuencias
+ Sin claves ni costos.
− Respetar la política de uso de teselas de OSM (uso personal moderado).
