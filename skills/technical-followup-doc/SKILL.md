---
name: "technical-followup-doc"
description: "Convierte contexto de reunión/implementación/incidente técnico en un documento estructurado en Google Docs."
---

# technical-followup-doc

Crea un documento estructurado en Google Docs a partir del contexto real que Pablete entrega sobre una reunión, implementación, incidente o seguimiento técnico. Descubre la acción de creación en runtime usando el MCP de Zapier. No inventa estado ni acuerdos.

## Input

- `títuloOTema` — nombre descriptivo del documento.
- `contexto` — qué originó la necesidad del documento.
- `estadoActual` — situación presente del tema.
- `acuerdosODecisiones` — lo que se acordó o decidió.
- `pendientes` — lo que queda por hacer.
- `próximosPasos` — acciones siguientes concretas.
- `audiencia` (opcional) — cliente, equipo interno, mixto.

Si faltan datos críticos sin los cuales se inventaría estado o acuerdos, preguntar antes de redactar.

## Descubrimiento de la acción Google Docs (runtime)

1. `inspect_zapier_actions()` sin argumentos → lista apps habilitadas con su `selected_api`.
2. Localizar Google Docs en la respuesta. Anotar su `selected_api` real.
3. `inspect_zapier_actions(selected_api: <valor_real>)` → lista acciones disponibles de Google Docs.
4. Identificar una acción que explícitamente permita **crear un documento** (create doc, create document, etc.). Anotar su `action` key y `tool_name`.
5. Si no existe esa acción: detenerse e informar a Pablete. No usar otra app ni acción distinta como sustituto.
6. Si existe: `inspect_zapier_actions(selected_api: <valor_real>, tool_name: <tool_name>, include_output_schema: true)` → obtener schema completo de parámetros requeridos y opcionales.

## Redacción

Redactar el contenido del documento con la voz de Pablete, adaptado a la audiencia:

- **Estilo:** profesional, cordial, directo, claro (USER.md). Sin relleno. Técnico pero comprensible.
- **Si es para clientes:** menos tecnicismos innecesarios (TOOLS.md).
- **Estructura del documento:**
  - Título descriptivo
  - Resumen / contexto
  - Estado actual
  - Acuerdos o decisiones tomadas
  - Pendientes
  - Próximos pasos

## Ejecución

`execute_zapier_write_action(selected_api: <valor_real>, action: <action_key>, tool_name: <tool_name>, params: { <según schema> })`

Los nombres de los parámetros se toman **del schema** obtenido en el paso 6. No se hardcodean.

## Post-ejecución

Confirmar a Pablete con el título del documento, un enlace o referencia al mismo, y el estado: creado, pendiente de revisión. No sobrescribir documentos existentes sin confirmación explícita.

## Constraints

- No sobrescribir documentos sin confirmar.
- No inventar estado, acuerdos ni decisiones. Si faltan datos críticos, preguntar.
- No hardcodear `selected_api`, `action`, `tool_name` ni nombres de parámetros. Todo se descubre en runtime.
- Si no existe acción de crear documento en Google Docs, informar y detener.
- No pedir APIs nuevas, OAuth ni conexiones. Solo MCP Zapier ya autorizado.
- No incluir secretos, tokens ni datos no proporcionados.
- No commitear, pushear ni publicar nada como parte de esta skill.
