---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-005: Un solo cerebro y un solo ejecutor de herramientas

## Contexto
El chat, la voz, los botones de las tarjetas y los recordatorios deben comportarse igual: mismas
reglas de confirmación, permisos y honestidad.

## Decisión
- `shared/tools.json` es la fuente única (33 herramientas): se empaqueta en `:core`, se envía al
  modelo de texto y a Realtime en `session.update`.
- `ToolExecutor`: validación de argumentos → permisos → confirmación → ejecución con timeout →
  resultado estructurado → registro.
- `JarvisBrain`: ruta local para intenciones claras (confianza ≥ 0,85) sin gastar IA; si no, bucle
  con el modelo limitado por `ModePolicy`. Sin servidor, respuesta honesta de lo que falta.
- Los botones de las tarjetas llaman al mismo ejecutor (`CardActionHandler`).

## Consecuencias
+ Un cambio de regla se aplica en todas partes.
+ Ahorro: hora, cálculos, tareas, apps y memoria no consumen tokens.
