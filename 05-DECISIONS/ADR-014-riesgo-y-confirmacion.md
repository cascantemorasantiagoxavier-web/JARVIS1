---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-014: Niveles de riesgo y política de confirmación

## Contexto
Las nuevas habilidades redactan correos y mensajes, publican, guardan archivos, mueven inventario y
manejan finanzas. Regla del usuario: nada de compras, envíos, eliminaciones ni acciones sensibles sin
autorización explícita.

## Decisión
- Cada herramienta y cada habilidad declara `risk`: `read`, `local_write`, `external`, `sensitive`.
- `ToolExecutor.needsConfirmation`: `sensitive` o `confirm: true` → siempre; `external` → siempre,
  salvo toque directo del usuario en un botón (no vale si la pidió una habilidad o el modelo).
- Nada se envía solo: correo = borrador `mailto:`; SMS/WhatsApp = la app con el texto; publicar =
  compartir; llamar = marcador. Los permisos SEND_SMS/CALL_PHONE no se declaran.
- Herramientas internas (`exposed: false`) solo accesibles para habilidades, no para el modelo directo.
- La confirmación muestra qué habilidad la pide y oculta argumentos largos.

## Consecuencias
+ Una instrucción maliciosa en una página o documento no puede enviar nada.
− Más toques para el usuario en flujos externos (aceptado).
