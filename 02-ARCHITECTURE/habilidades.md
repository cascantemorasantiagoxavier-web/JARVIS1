---
tipo: catalogo
actualizado: 2026-10-23
generado: JARVIS/scripts/skills/catalog.py
---
# Catálogo de habilidades

Generado desde `JARVIS/shared/skills/*.json` y las pruebas de `:core`. **No editar a mano**: ejecutar
`python3 -I scripts/skills/catalog.py --write`.

El estado depende de la configuración del teléfono (la app lo calcula en vivo en la pantalla
*Skills*). "Sin probar en teléfono" lista las partes que dependen de Android real (cámara, micrófono, selector,
intents) y que por eso siguen NO TESTEADO. "Pruebas específicas" cuenta las pruebas automáticas que ejecutan esa habilidad además de
las genéricas de contrato. Todas son AUTOMATIZADO con datos simulados: verifican la lógica, no los
servicios reales (OpenAI, OSM, cámara, micrófono), que quedan NO TESTEADO hasta probar en un teléfono.

Cómo añadir una habilidad: `JARVIS/docs/SKILLS.md`. Decisiones: [[05-DECISIONS/ADR-013-sistema-de-habilidades|ADR-013]],
[[05-DECISIONS/ADR-014-riesgo-y-confirmacion|ADR-014]].

Total: 45 habilidades · pruebas genéricas de contrato para todas: 13 (SkillContractTest)

Resumen sin configurar nada: IMPLEMENTADA 30, PARCIAL 7, REQUIERE_CONFIGURACIÓN 8

Resumen con servidor y bóveda: IMPLEMENTADA 37, PARCIAL 8

| Habilidad | Categoría | Riesgo | Sin configurar | Con servidor y bóveda | Pendiente | Pruebas específicas | Sin probar en teléfono |
|---|---|---|---|---|---|---|---|
| `barcode_business_card_scanner` Escáner de códigos y tarjetas | medios | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 3 | cámara |
| `business_prospecting` Prospección de negocios | negocio | modifica local | REQUIERE_CONFIGURACIÓN (requiere servidor) | IMPLEMENTADA | — | 1 | ubicación |
| `cashflow_forecast` Proyección de flujo de caja | finanzas | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `competitor_intelligence` Inteligencia competitiva | negocio | lectura | REQUIERE_CONFIGURACIÓN (requiere servidor) | IMPLEMENTADA | — | 1 | — |
| `contract_review` Revisión de contratos | negocio | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | selector de archivos |
| `conversation_memory` Memoria persistente | asistente | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 1 | guardar archivo |
| `customer_relationships` Relación con clientes (CRM) | negocio | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 1 | avisos |
| `daily_briefing` Resumen del día | asistente | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 3 | calendario |
| `deep_research` Investigación profunda | investigación | lectura | REQUIERE_CONFIGURACIÓN (requiere servidor) | IMPLEMENTADA | — | 1 | — |
| `development_copilot` Copiloto de programación | productividad | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `device_diagnostics` Diagnóstico del teléfono | asistente | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | estado del teléfono |
| `document_reader` Lector de documentos | productividad | lectura | PARCIAL | PARCIAL | PDF (no hay librería de extracción incluida); documentos escaneados (usar visual_scene_assistant con una foto) | 3 | selector de archivos |
| `email_assistant` Asistente de correo | comunicación | acción externa | PARCIAL | PARCIAL | leer la bandeja de entrada (requiere OAuth de Gmail/Outlook o IMAP; no configurado); enviar directamente (por diseño, el envío lo hace el usuario) | 1 | correo |
| `expense_capture` Registro de gastos e ingresos | finanzas | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 3 | cámara |
| `home_inventory` Inventario del hogar | hogar | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `import_export_research` Investigación de importación y exportación | negocio | lectura | REQUIERE_CONFIGURACIÓN (requiere servidor) | IMPLEMENTADA | — | 3 | — |
| `invoice_quote_creator` Cotizaciones y proformas | finanzas | acción externa | PARCIAL | PARCIAL | factura electrónica oficial de Hacienda (requiere firma digital y la API de comprobantes electrónicos); adjuntar el PDF al correo (se adjunta el HTML a mano) | 1 | correo, guardar archivo |
| `language_coach` Entrenador de idiomas | aprendizaje | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `maps_navigation` Mapas y navegación | asistente | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | mapa |
| `marketing_optimizer` Optimizador de marketing | marketing | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `meal_and_grocery_planner` Comidas y compras | hogar | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `meeting_assistant` Asistente de reuniones | asistente | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 1 | avisos, bóveda (SAF), calendario, guardar archivo |
| `message_drafter` Redactor de mensajes | comunicación | acción externa | IMPLEMENTADA | IMPLEMENTADA | — | 2 | SMS/WhatsApp, compartir, correo |
| `messaging_assistant` Asistente de mensajería | comunicación | acción externa | PARCIAL | PARCIAL | leer o enviar mensajes automáticamente (Android y WhatsApp no lo permiten a otras apps) | 1 | SMS/WhatsApp, avisos |
| `negotiation_coach` Entrenador de negociación | negocio | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `obsidian_memory` Memoria de Obsidian | conocimiento | lectura | REQUIERE_CONFIGURACIÓN (requiere bóveda) | IMPLEMENTADA | — | 1 | bóveda (SAF) |
| `personal_finance` Finanzas personales | finanzas | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 2 | — |
| `personal_tutor` Tutor personal | aprendizaje | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 2 | — |
| `price_comparison` Comparación de precios | compras | lectura | REQUIERE_CONFIGURACIÓN (requiere servidor) | IMPLEMENTADA | — | 1 | — |
| `renewal_monitor` Monitor de renovaciones | asistente | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 1 | avisos |
| `report_generator` Generador de informes | productividad | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 1 | guardar archivo |
| `routine_automation` Rutinas | automatización | acción externa | PARCIAL | PARCIAL | disparadores automáticos por hora o ubicación (hoy se ejecutan a petición) | 1 | según los pasos |
| `sales_coach` Entrenador de ventas | negocio | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 2 | avisos |
| `schedule_optimizer` Optimizador de agenda | asistente | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | calendario |
| `social_media_manager` Gestor de redes sociales | marketing | acción externa | PARCIAL | PARCIAL | publicar directamente (requiere conectar cada red con su API, permisos y revisión de la app); métricas de las publicaciones | 1 | avisos, compartir |
| `spreadsheet_analyst` Analista de hojas de cálculo | productividad | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 3 | selector de archivos |
| `stock_control` Control de inventario del negocio | negocio | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 3 | — |
| `supplier_verifier` Verificador de proveedores | negocio | lectura | IMPLEMENTADA | IMPLEMENTADA | — | 1 | — |
| `task_manager` Gestor de tareas | asistente | sensible | IMPLEMENTADA | IMPLEMENTADA | — | 2 | avisos |
| `travel_planner` Planificador de viajes | asistente | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 1 | avisos |
| `video_clips` Clips de video y YouTube | marketing | sensible | REQUIERE_CONFIGURACIÓN (requiere servidor) | PARCIAL | contrato de la API de OpusClip tomado de un conector de terceros (la documentación oficial no fue accesible): confirmar con la primera prueba real; aviso automático cuando los clips están listos (hoy se pregunta a JARVIS) | 3 | — |
| `visual_scene_assistant` Asistente visual | medios | lectura | REQUIERE_CONFIGURACIÓN (requiere servidor) | IMPLEMENTADA | — | 1 | cámara |
| `voice_memo_inbox` Bandeja de notas de voz | productividad | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 3 | avisos, guardar archivo, micrófono |
| `website_builder` Creador de sitios web | marketing | modifica local | PARCIAL | PARCIAL | publicación automática en un hosting (se entrega el archivo HTML) | 1 | guardar archivo |
| `writing_editor` Editor de textos | productividad | modifica local | IMPLEMENTADA | IMPLEMENTADA | — | 4 | guardar archivo |
