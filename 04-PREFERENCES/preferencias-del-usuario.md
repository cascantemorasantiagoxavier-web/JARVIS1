---
tipo: preferencias
actualizado: 2026-10-23
---
# Preferencias expresadas por el usuario

Solo lo que el usuario dijo o mostró. No añadir suposiciones.

| Preferencia | Origen |
|---|---|
| Idioma: español; contexto Costa Rica (provincias, cantones, distritos) | Superprompt maestro |
| Voz principal: OpenAI GPT Realtime por WebRTC, con interrupciones y herramientas | Superprompt maestro |
| Activación por la palabra "JARVIS" | Superprompt maestro |
| Interfaz: HUD ámbar/naranja sobre negro según la imagen de referencia (marca, reloj hexagonal, clima, panel de apps, globo holográfico con anillos y pedestal, actividades, casco, barra de voz `Di: "Jarvis"`) | Imagen enviada ("AHÍ ESTÁ") |
| El mapa no debe ser la pantalla principal; solo bajo petición | Superprompt maestro |
| Integración con su bóveda de Obsidian | Superprompt maestro |
| Tiene muchas claves de API ("28 APIS KEYS PARA JARVIS"). Deben ir en `backend/.env` del servidor o en los secretos del hosting, **nunca** en el chat ni aquí. Dio los nombres el 2026-10-23: UN Comtrade, Google Maps y APIs de Google Cloud; análisis en [[07-RESEARCH/apis-del-usuario]]. No incluyen OpenAI. | Mensaje del usuario |
| Repositorios en GitHub (`cascantemorasantiagoxavier-web`): `JARVIS` (proyecto anterior, rama `master`), `JARVIS2`, `JARVIS-backend`, todos públicos. Autorizó subir el proyecto («haz lo que quieras»); se usará la rama `jarvis-v0.1-habilidades` sin tocar `master` | Mensajes del usuario |
| Quiere un sistema de habilidades amplio (44: 14 + 30) y skills de Claude Code para el desarrollo, con el estado verdadero de cada una (IMPLEMENTADA / PARCIAL / REQUIERE_CONFIGURACIÓN / NO_IMPLEMENTADA) | «Extensión maestra» y «30 skills adicionales» |
| Prioridad: primero lo de valor inmediato (editor, analista de archivos, informes, ventas, inventario local); después lo que depende de permisos y conectores | «30 skills adicionales» |
| Autoriza trabajo autónomo prolongado («me voy a dormir») | Mensaje del usuario |

## Pendiente de confirmar con el usuario
- Clave de OpenAI (imprescindible) y, si quiere búsqueda web completa, Tavily o Brave.
- Dónde alojará el servidor (ver `JARVIS/docs/HOSTING.md`).
- Instalar la app de Claude en GitHub para poder subir (la subida dio 403).
- Modelo de teléfono y versión de Android para las pruebas en dispositivo.
- Si quiere conectar correo (Gmail/Outlook por OAuth), redes sociales o factura electrónica: cada uno
  requiere sus credenciales en el servidor y una decisión (hoy son PARCIALES por diseño).
