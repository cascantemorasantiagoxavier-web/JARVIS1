---
tipo: investigacion
actualizado: 2026-10-23
---
# Limitaciones del entorno de desarrollo (2026-10-23)

El proyecto se construyó en un contenedor en la nube con una política de red restringida.

| Servicio | Acceso | Efecto |
|---|---|---|
| Maven Central, plugins.gradle.org | Sí (con errores 429 ocasionales) | `:core` compila y se prueba |
| npm, PyPI | Sí | Backend completo |
| `dl.google.com` / `maven.google.com` | **Bloqueado** | Sin Android Gradle Plugin, AndroidX ni Compose → no hay APK |
| `api.openai.com`, documentación de OpenAI | **Bloqueado** | Contrato verificado con el SDK npm; sin pruebas reales |
| `github.com` | **Bloqueado** | Sherpa-ONNX sin descargar ni compilar |
| Overpass, Nominatim, Wikipedia, DuckDuckGo | **Bloqueado** | Pruebas en vivo pendientes |
| Emulador / dispositivo | No disponible | Audio, WebRTC, permisos y UI sin probar |

## Cómo desbloquear
- En una máquina propia con Android Studio, todo lo anterior es accesible.
- O pedir al administrador del entorno que permita esos dominios en la política de red.

No se usaron espejos de terceros para saltarse el bloqueo.
