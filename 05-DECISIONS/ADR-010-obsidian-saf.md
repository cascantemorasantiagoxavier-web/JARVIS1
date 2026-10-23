---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-010: Bóveda de Obsidian por SAF, solo lectura, con notas privadas excluidas

## Contexto
El usuario quiere que JARVIS use su bóveda, pero sin exponer notas privadas al modelo.

## Decisión
- Acceso con el selector de carpetas del sistema (SAF), permiso persistente, solo lectura.
- Índice local (Room + FTS4 sin tildes), incremental, cada 12 h con WorkManager.
- Privadas: carpeta `Privado/`, etiquetas `#privado`/`#confidencial`/`#no-jarvis`, frontmatter
  `privado: true`. Nunca se envían al modelo; sí se pueden ver localmente.
- Contexto automático de notas desactivado por defecto (`autoNotesContext = false`): las notas solo
  llegan al modelo cuando se usa `search_notes`, como fragmentos relevantes con su ruta.

## Consecuencias
+ Privacidad por diseño. − Sin escritura en la bóveda (posible fase futura).
