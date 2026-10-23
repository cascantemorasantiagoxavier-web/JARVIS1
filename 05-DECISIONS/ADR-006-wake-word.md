---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-006: Palabra clave "JARVIS" local con Sherpa-ONNX, opcional

## Contexto
La activación por voz debe funcionar sin enviar audio a la nube y sin costo. Opciones evaluadas:
Sherpa-ONNX (Apache-2.0, palabras clave abiertas sin reentrenar), Porcupine (requiere clave de
acceso y licencia comercial para ciertos usos), openWakeWord (modelos por palabra; integración
Android menos directa).

## Decisión
- Sherpa-ONNX `KeywordSpotter` con un modelo zipformer de GigaSpeech (inglés; "JARVIS" es una
  palabra inglesa).
- Se activa solo si existe `app/libs/sherpa-onnx*.aar` (source set `sherpa`); si no, el source set
  `nosherpa` explica que no está disponible.
- `scripts/fetch-sherpa-kws.sh` descarga el AAR y el modelo y genera `keywords.txt` con el BPE del
  modelo.
- Servicio en primer plano tipo `microphone` con notificación y botón Detener, solo mientras el
  usuario la tenga activada.

## Consecuencias
− Escrito sin poder compilar contra el AAR (sin acceso a GitHub): NO VERIFICADO.
− Consumo de batería a medir en dispositivo.
