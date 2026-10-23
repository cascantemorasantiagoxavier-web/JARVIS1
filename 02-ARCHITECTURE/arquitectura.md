---
tipo: arquitectura
actualizado: 2026-10-23
---
# Arquitectura general

```
┌──────────────────────── Teléfono Android ────────────────────────┐
│  UI Compose (HUD)   Chat   Voz (VoiceManager)   Recordatorios     │
│        │              │          │                    │           │
│        └──────────────┴──► JarvisOrchestrator ◄───────┘           │
│                              │                                     │
│                         JarvisBrain ── IntentDetector (reglas ES)  │
│                              │  ruta local (sin IA) o modelo       │
│                         ToolExecutor (validación, permisos,        │
│                              │        confirmación, registro)      │
│      CoreToolset (45 herramientas) ◄── SkillEngine (44 habilid.)   │
│     Room · DataStore · Keystore · AlarmManager · Intents · SAF     │
│                                                                    │
│  RealtimeSessionController ◄── data channel "oai-events" ──┐       │
│  WebRtcTransport (audio con AEC/NS) ════ audio WebRTC ═══╗ │       │
└────────────────────────────│───────────────────────────║─│───────┘
                             │ HTTPS + token              ║ │
                ┌────────────▼──────────────┐             ║ │
                │  Servidor JARVIS (Node)   │             ║ │
                │  clave de OpenAI (.env)   │──SDP──┐     ║ │
                │  /api/chat  /api/web/*    │       ▼     ║ │
                │  /api/business/* /api/vision /api/audio │ OpenAI Realtime
                └──┬─────────┬──────────┬───┘   (gpt-realtime-2.1-mini)
                   │         │          │
            OpenAI Responses │   Overpass + Nominatim (OSM)
                    SearXNG/Tavily/Brave/Wikipedia
```

## Módulos
- **`:core`** (Kotlin/JVM puro): toda la lógica probada sin Android. Ver [[05-DECISIONS/ADR-001-modulos-core-app]].
- **`:app`** (Android): adaptadores de plataforma y UI.
- **`backend/`**: única pieza con secretos de proveedores.
- **`shared/`**: `tools.json` (fuente única de herramientas) y `persona-es.md`.

## Decisiones clave
- Un solo cerebro y ejecutor para chat, voz y botones → [[05-DECISIONS/ADR-005-cerebro-unico]].
- Realtime "unificado" por defecto → [[05-DECISIONS/ADR-002-realtime-unificado]].
- Estrategias de respuesta: AUTO, FAST, BALANCED, DEEP (`ModePolicy`: rondas de herramientas,
  historial y si se prefiere la ruta local).

## Persistencia (Room)
`memories`, `tasks`, `prospects` (businessId único, instantánea JSON de la ficha), `chat_entries`
(con tarjetas serializadas), `tool_log` (30 días), `note_chunks` + `note_chunks_fts` (FTS4 `unicode61
remove_diacritics`: búsqueda sin tildes).

## Ver también
- [[02-ARCHITECTURE/flujo-de-voz]] · [[02-ARCHITECTURE/herramientas]] · [[02-ARCHITECTURE/interfaz]]

## Habilidades (desde 2026-10-23)
Capa sobre las herramientas: cada habilidad es un manifiesto JSON + una clase en `:core`. El
`SkillEngine` valida entrada y salida, comprueba configuración y permisos, aplica tiempo límite y
cancelación, y solo actúa mediante el mismo `ToolExecutor` (con niveles de riesgo y confirmaciones).
El modelo ve un catálogo breve y usa `list_skills` / `describe_skill` / `run_skill`; sin servidor,
`SkillRouter` resuelve en local las frases claras. Datos en Room (`skill_records`).
Decisiones: [[05-DECISIONS/ADR-013-sistema-de-habilidades|ADR-013]],
[[05-DECISIONS/ADR-014-riesgo-y-confirmacion|ADR-014]], [[05-DECISIONS/ADR-015-vision-y-transcripcion|ADR-015]].
Estado de cada una: [[02-ARCHITECTURE/habilidades|catálogo]]. Guía para añadir: `JARVIS/docs/SKILLS.md`.
