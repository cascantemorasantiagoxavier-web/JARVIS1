---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-012: Gestión de secretos

## Contexto
Regla del usuario: la clave de OpenAI nunca en código, Gradle, recursos, APK, repositorio,
Obsidian ni logs.

## Decisión
- Claves de proveedores solo en `backend/.env` (en `.gitignore`) o secretos del hosting.
- La app guarda la URL del servidor y `JARVIS_CLIENT_TOKEN` cifrado (AES-GCM, clave en Android
  Keystore). Copias de seguridad desactivadas.
- El backend compara el token en tiempo constante y redacta secretos en los logs; una prueba
  comprueba que ninguna respuesta contiene la clave.

## Consecuencias
+ Un APK filtrado no expone la cuenta de OpenAI. − Si se filtra el token, hay que rotarlo en
  servidor y app.
