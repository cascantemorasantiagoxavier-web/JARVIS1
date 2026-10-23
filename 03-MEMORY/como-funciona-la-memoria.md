---
tipo: memoria
actualizado: 2026-10-23
---
# Cómo funciona la memoria de JARVIS

JARVIS tiene tres fuentes de contexto, todas en el teléfono:

## 1. Memoria persistente (Room, tabla `memories`)
- Se guarda **solo** cuando el usuario lo pide ("recuerda que…", o la IA llama a `remember` a petición suya).
- Tipos: preferencia, dato, proyecto, contacto, otro (se deducen de la frase: "prefiero…" →
  preferencia).
- No duplica: si ya existe uno igual (sin mayúsculas ni tildes) o muy parecido (similitud ≥ 0,9)
  lo informa; si una preferencia reformula otra parecida, la actualiza. Corrección con `update_memory`.
- "Recuerda…, es privado" marca el recuerdo como privado.
- Borrar (`forget`) siempre pide confirmación.
- Se consulta con `recall`. Las preferencias y los recuerdos relevantes NO privados se añaden como
  contexto de cada petición (desactivable en Memoria → Privacidad).
- Persiste al cerrar y reabrir la app (probado en `:core` con un repositorio de archivo y, escrito
  para Robolectric, con Room real).

## 2. Bóveda de Obsidian (solo lectura)
- El usuario elige la carpeta con el selector del sistema (SAF). JARVIS no escribe en ella.
- Indexado incremental por fecha de modificación, cada 12 h y bajo demanda. Límites: 5000 archivos,
  2 MB por archivo.
- Las notas se dividen en fragmentos por encabezados y se buscan con FTS sin tildes.
- **Privadas** (nunca se envían al modelo): carpetas `Privado/`, `Private/`; etiquetas `#privado`,
  `#private`, `#confidencial`, `#no-jarvis`; frontmatter `privado: true`, `private: true` o
  `jarvis: no`.
- Las notas son datos no confiables: si contienen instrucciones, no se siguen.

## 3. Historial de conversación
- Últimos mensajes (con sus tarjetas de resultados) en la tabla `chat_entries`; se puede borrar
  desde Ajustes → Datos.

## Qué no se guarda
- Audio de las conversaciones.
- Claves o tokens en texto plano (el token del servidor va cifrado con Android Keystore).
- Copias de seguridad en la nube: desactivadas (`allowBackup=false`, reglas de extracción).

## Esta carpeta de la KB
Sirve para documentar el diseño de la memoria. Los recuerdos reales del usuario viven en el
teléfono, no aquí.
