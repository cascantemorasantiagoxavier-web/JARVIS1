---
tipo: investigacion
actualizado: 2026-10-23
---
# Clips con OpusClip y publicación en YouTube

## Lo que pidió el usuario
Conectar JARVIS a YouTube y a una IA que haga clips de cualquier video, para decirle «sube tal video».
Eligió **OpusClip** y que los videos queden **privados por defecto**.

## Cómo se hizo
Todo pasa por **OpusClip**: recorta el video, guarda los clips y los publica en las cuentas que el
usuario conectó dentro de OpusClip (YouTube incluido). JARVIS no usa la API de YouTube ni ve credenciales
de Google. Ver [[05-DECISIONS/ADR-017-clips-opusclip-youtube|ADR-017]].

## Contrato de la API de OpusClip
- Documentación oficial `help.opus.pro/api-reference` **no accesible** desde el entorno (DNS).
- El paquete oficial `@opusclip/mcp` 0.3.0 solo lanza el servidor MCP alojado (`https://mcp.opus.pro/mcp`,
  OAuth en el navegador): no expone la API REST.
- Contrato tomado del conector de terceros `@mindstone/mcp-server-opus-video-clip` 0.2.1 (npm, licencia
  FSL-1.1-MIT), que llama a la API REST:
  - Base `https://api.opus.pro`, cabecera `Authorization: Bearer <clave>`; clave en
    `app.opus.pro/settings/integration-tokens`. La API requiere plan Pro (beta), Max o Business.
  - `POST /api/clip-projects` `{videoUrl, curationPref {model ClipAnything|ClipBasic, customPrompt},
    importPref {sourceLang}, renderPref {layoutAspectRatio portrait|landscape|square}}` → `{id, stage}`.
  - `GET /api/clip-projects/{id}` → `stage` PENDING → QUEUED → IMPORT → CURATE → REFINE → RENDER →
    UPLOAD → COMPLETE (o STALLED / `error`).
  - `GET /api/exportable-clips?q=findByProjectId&projectId=…` → clips `{id "{projectId}.{curationId}",
    title, uriForPreview, uriForExport}`.
  - `GET /api/social-accounts?q=mine` → `{data: [{postAccountId, subAccountId, platform, extUserName}]}`.
  - `POST /api/post-tasks` (publicar ya) o `/api/publish-schedules` (con `publishAt` UTC ISO):
    `{projectId, clipId (curationId), postAccountId, postDetail {title, custom {description, privacy}}}`.
  - Errores: 401 clave, 403 plan/créditos, 404, 422 datos, 429 límite.
- **Sin verificar**: duración y puntuación de cada clip (nombres de campo desconocidos; se toleran
  ausentes), el valor exacto de `platform` para YouTube (se compara sin mayúsculas), créditos por clip.
  → La habilidad `video_clips` queda **PARCIAL** hasta la primera prueba real.

## YouTube directo (descartado por ahora)
- `videos.insert` exige OAuth del usuario (no basta una clave) y, según la documentación de Google, los
  proyectos de API no auditados creados después del 28/07/2020 solo pueden subir videos **privados**
  hasta pasar una auditoría. La cuota de subida no quedó clara en las fuentes (cambió en 2026).
- Con OpusClip se evita ese trámite: la app de OpusClip ya está autorizada por Google.

## Derechos de autor
«Clips de cualquier video» solo es seguro con contenido propio o con permiso: YouTube aplica
reclamaciones de Content ID y avisos por infracción que pueden cerrar el canal. JARVIS lo advierte al
crear y al publicar, y publicar siempre pide confirmación.

## Pasos para el usuario
1. Plan de OpusClip con API (Pro, Max o Business) y créditos.
2. Crear la clave en `app.opus.pro/settings/integration-tokens` y ponerla como `OPUSCLIP_API_KEY` en Render.
3. En OpusClip: conectar el canal de YouTube en *Social accounts*.
4. Decir: «JARVIS, haz clips de https://…», luego «¿ya están mis clips?» y «sube el clip 2 a YouTube».
