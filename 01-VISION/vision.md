---
tipo: vision
actualizado: 2026-10-23
---
# Visión

Un asistente personal real, en español de Costa Rica, con la presencia del JARVIS de Iron Man:
conversación por voz natural, que actúa con herramientas reales y que nunca inventa.

## Principios
1. **Útil de verdad**: hace cosas (tareas, avisos, búsquedas, prospectos, apps), no solo conversa.
2. **Honesto**: si un dato no existe en la fuente, dice "no disponible". Cada dato con su fuente.
3. **Privado por defecto**: la clave de OpenAI solo en el servidor; notas privadas nunca salen del
   teléfono; la memoria guarda solo lo que el usuario autoriza.
4. **Bajo costo**: modelo mini por defecto, ruta local sin IA cuando basta, límites y cierre por
   inactividad, consumo visible.
5. **Un solo cerebro**: chat, voz, botones y automatizaciones usan las mismas herramientas y reglas.
6. **El usuario manda**: nada de mensajes, llamadas o borrados sin su confirmación.

## Alcance de la versión 0.1
- App Android (Kotlin, Compose) + servidor propio (Node).
- Voz GPT Realtime por WebRTC con interrupciones y herramientas; voz local de respaldo.
- Palabra clave "JARVIS" local (opcional, Sherpa-ONNX).
- Memoria persistente, bóveda de Obsidian de solo lectura.
- Búsqueda web con fuentes; negocios de Costa Rica (OpenStreetMap) con estado por dato; prospectos.
- Tareas, recordatorios, calendario (lectura + crear vía app de calendario), clima, apps.
- Interfaz HUD ámbar según la imagen de referencia del usuario; mapa solo bajo petición.

## Fuera de alcance (por ahora)
- Enviar mensajes, correos o hacer llamadas automáticamente.
- Compras o pagos.
- Escribir en la bóveda de Obsidian.
- iOS.
