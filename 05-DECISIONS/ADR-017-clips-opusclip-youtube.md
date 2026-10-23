---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-017: Clips con OpusClip y publicación en YouTube a través de OpusClip

## Contexto
El usuario quiere pedir por voz «haz clips de este video» y «sube tal clip a YouTube». Eligió OpusClip
y privacidad privada por defecto. Subir directo con la API de YouTube exige OAuth y, sin auditoría de
Google, deja los videos en privado ([[07-RESEARCH/opusclip-youtube|investigación]]).

## Decisión
- El servidor habla con OpusClip (`/api/clips/projects`, `/api/clips/projects/:id`, `/api/clips/accounts`,
  `/api/clips/publish`) con `OPUSCLIP_API_KEY`; límites diarios de proyectos (créditos) y publicaciones.
- La publicación usa la cuenta de YouTube conectada **dentro de OpusClip**; si hay varias, hay que
  elegir; si no hay ninguna, se explica cómo conectarla. JARVIS nunca maneja credenciales de Google.
- Herramientas internas: `clips_create` (riesgo externo: gasta créditos → confirmación),
  `clips_status` (lectura), `clips_publish` (**sensible**: confirmación siempre, también por voz).
- Habilidad `video_clips`: guarda los proyectos para que «sube el clip 2» funcione sin ids; título del
  clip por defecto, **privado por defecto**, programación con fechas en español («mañana a las 6 pm»).
- Aviso de derechos de autor al crear y al publicar.
- Estado **PARCIAL**: el contrato de la API viene de un conector de terceros (la documentación oficial no
  fue accesible) y no hay aviso automático cuando los clips terminan.

## Consecuencias
+ Sin auditoría de Google ni OAuth propio; un solo proveedor y una sola clave.
− Depende del plan y los créditos de OpusClip, y de que su API no cambie.
− La primera prueba real debe confirmar nombres de campos (duración, puntuación, plataforma).
