---
tipo: adr
estado: Aceptada
fecha: 2026-10-23
---
# ADR-013: Sistema de habilidades declarativo sobre el contrato único de herramientas

## Contexto
El usuario pidió 44 habilidades (14 + 30) con registro declarativo, estado verdadero, conexión a
GPT Realtime sin enviar todo el catálogo en cada turno, y sin ejecución de código arbitrario.
Ya existía un único `ToolExecutor` para chat, voz y botones ([[05-DECISIONS/ADR-005-cerebro-unico|ADR-005]]).

## Decisión
- Cada habilidad = **manifiesto JSON** (`JARVIS/shared/skills/<id>.json`, empaquetado como recurso)
  + **clase Kotlin** en `:core` que implementa `Skill`. El manifiesto declara ejemplos, esquemas de
  entrada/salida, herramientas, permisos, servicios, costo, riesgo, límites, cancelación, errores,
  madurez (`implemented`/`partial` con `missing`) y casos de prueba.
- `SkillRegistry` rechaza manifiestos con herramientas inexistentes o con riesgo menor que el de sus
  herramientas, y calcula el estado (IMPLEMENTADA / PARCIAL / REQUIERE_CONFIGURACIÓN / NO_IMPLEMENTADA)
  según la configuración actual.
- `SkillEngine` valida la entrada y la salida contra los esquemas, comprueba configuración y permisos,
  aplica tiempo límite, tope de llamadas a herramientas y cancelación, y registra cada ejecución.
- Las habilidades solo actúan mediante `SkillContext.tool()` con las herramientas que declararon:
  pasan por el mismo `ToolExecutor` (permisos, confirmaciones, registro).
- Modelo: tres meta-herramientas (`list_skills`, `describe_skill`, `run_skill`) y un **catálogo breve**
  en las instrucciones; los esquemas completos solo bajo demanda. Sin modelo, `SkillRouter` ejecuta en
  local las frases que coinciden con claridad.
- Datos estructurados en un almacén genérico por colecciones (`RecordStore`; Room `skill_records` en
  Android, migración 1→2) en vez de una tabla por habilidad.

## Consecuencias
+ Añadir una habilidad no toca el cerebro ni la voz; las pruebas de contrato cubren a todas por igual.
+ El estado mostrado al usuario y al modelo sale del mismo cálculo.
− Los esquemas JSON se validan con un validador propio (subconjunto de JSON Schema).
− Datos en JSON por colección: consultas complejas se hacen en memoria (volúmenes personales).
