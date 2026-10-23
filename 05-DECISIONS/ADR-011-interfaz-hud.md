---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-011: Interfaz HUD ámbar fiel a la imagen de referencia

## Contexto
El usuario envió una imagen con el diseño deseado.

## Decisión
- Compose con componentes propios (`HudPanel`, `ChamferShape`, `HudButton`…), fuentes OFL Orbitron
  y Rajdhani, paleta ámbar y alternativa cian.
- Núcleo holográfico propio dibujado en Canvas (globo de puntos de Natural Earth), sin librerías 3D.
- Diseño horizontal de tres columnas y vertical con pestañas.

## Consecuencias
+ Sin dependencias gráficas pesadas. − Aspecto real pendiente de ver en dispositivo.
