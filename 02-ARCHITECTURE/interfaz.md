---
tipo: arquitectura
actualizado: 2026-10-23
---
# Interfaz HUD

Basada en la imagen de referencia que envió el usuario: HUD ámbar/naranja sobre negro.

## Disposición horizontal o pantalla ancha (≥ 720 dp)
| Zona | Contenido |
|---|---|
| Fila superior | Marca "J.A.R.V.I.S.", reloj en marco hexagonal, clima (Open-Meteo) |
| Columna izquierda | Panel de aplicaciones favoritas (configurables) y accesos a módulos |
| Centro | Núcleo holográfico: globo terrestre de puntos con anillos y pedestal; recuadro de actividad |
| Columna derecha | Últimas actividades; panel con casco y barra de estado |
| Fila inferior | Estado del sistema, barra de voz `Di: "Jarvis"`, frase |

## Disposición vertical
Mismo núcleo arriba y pestañas: **Apps / Actividad / Sistema / JARVIS**.

## Núcleo holográfico (`ui/core/HoloGlobe.kt`)
- Esfera de Fibonacci filtrada con la máscara de tierra de Natural Earth (0,5°), proyección
  ortográfica, inclinación 0,34 rad. Puntos dibujados con `drawPoints` nativo por cubetas de
  profundidad, sin asignar memoria por fotograma (`GlobeBuffers`).
- Arcos entre capitales. Anima según el estado: en reposo, escuchando, pensando, hablando, error.
- `reduceMotion` reduce la animación.
- Vista previa generada por la prueba `GlobeTest`: `JARVIS/app/Android/core/build/globe-preview.png`.

## Estilo
- Paleta `HudPalette` ámbar (alternativa cian en Ajustes). Fuentes Orbitron (títulos) y Rajdhani
  (texto), ambas SIL OFL.
- Paneles con esquinas biseladas (`ChamferShape`), borde luminoso y segmentos de acento.

## Pantallas
Inicio · Chat (estrategias AUTO/FAST/BALANCED/DEEP) · Web · Negocios · Mapa (solo bajo petición,
osmdroid con colores invertidos) · Tareas · Memoria (y bóveda) · Ajustes.

## Estado
Código completo, verificado con un arnés de tipos offline. **No se ha visto en un teléfono ni en un
emulador**: el aspecto real está NO TESTEADO.
