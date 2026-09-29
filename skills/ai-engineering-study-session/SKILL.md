---
name: "ai-engineering-study-session"
description: "Prepara sesiones de estudio personalizadas para Pablete y crea evento en Google Calendar solo tras confirmación explícita."
---

# ai-engineering-study-session

Prepara una sesión de estudio estructurada y progresiva para Pablete según el tema, objetivo y disponibilidad que indique. Primero propone la sesión con agenda; solo después de confirmación explícita crea el evento en Google Calendar. Usa el MCP de Zapier, descubriendo acciones en runtime.

## Fase 1: Input y propuesta

### Input

- `tema` — qué estudiar (concepto, tecnología, tema específico).
- `objetivo` — qué se espera lograr en la sesión.
- `duracion` — tiempo estimado (ej: 60 min, 90 min, 2 h).
- `fechaOVentana` — fecha concreta o ventana deseada (ej: jueves en la tarde, mañana 10:00, "esta semana").
- `notasORestricciones` (opcional) — preferencias de horario, material base, formato, etc.

Si falta tema, objetivo o duración, pedir lo mínimo.

### Propuesta (mostrar a Pablete)

Redactar:

- **Título del evento** — descriptivo y en español.
- **Fecha/hora propuesta** — en America/Santiago. Si Pablete dio una ventana, elegir un horario razonable dentro de ella. Si dio fecha concreta, usarla.
- **Duración** — la indicada.
- **Objetivo** — claro y concreto.
- **Agenda breve** — etapas progresivas de estudio: comenzar con fundamentos/contexto, avanzar en práctica o ejemplos, cerrar con resumen o dudas. Sin relleno, didáctico.

Incluir al final:

> **¿Confirmas que cree este evento en tu calendario?** Dame el OK y lo creo.

No crear el evento aún. Detenerse.

## Fase 2: Confirmación y creación del evento

Solo cuando Pablete confirme explícitamente:

1. `inspect_zapier_actions()` sin argumentos → lista apps habilitadas con su `selected_api`.
2. Localizar Google Calendar en la respuesta. Anotar su `selected_api` real.
3. `inspect_zapier_actions(selected_api: <valor_real>)` → lista acciones disponibles de Calendar.
4. Identificar una acción explícita y estructurada para **crear un evento** (Create Event, create event). Priorizar esta como primera opción. Anotar su `action` key y `tool_name`.
5. Si no existe una acción estructurada de creación, evaluar **Quick Add** como alternativa de respaldo solo si sus parámetros permiten establecer fecha, hora, duración y zona horaria sin ambigüedad. Si Quick Add no garantiza esos datos, detenerse e informar. Si tampoco existe Quick Add, detenerse e informar. No usar otra app ni acción distinta.
6. Si existe la acción elegida: `inspect_zapier_actions(selected_api: <valor_real>, tool_name: <tool_name>, include_output_schema: true)` → obtener schema completo de parámetros requeridos y opcionales.
7. Ejecutar `execute_zapier_write_action(selected_api: <valor_real>, action: <action_key>, tool_name: <tool_name>, params: { <según schema> })`.

Los nombres de los parámetros se toman **del schema** obtenido. No se hardcodean.

Si el schema requiere `calendar_id` o similar, no inventarlo: resolverlo mediante `inspect_zapier_actions` con `enum_property` o preguntar a Pablete.

### Post-creación

Confirmar a Pablete con:

- Título del evento.
- Fecha, hora y duración.
- Estado: creado en Google Calendar, pendiente de revisión si necesita ajustes.

## Constraints

- No crear el evento hasta recibir confirmación explícita de Pablete.
- No modificar ni cancelar eventos existentes.
- No hardcodear `selected_api`, `action`, `tool_name` ni nombres de parámetros. Todo se descubre en runtime.
- Priorizar acción estructurada de Create Event sobre Quick Add. Quick Add solo como respaldo si garantiza fecha, hora, duración y zona horaria sin ambigüedad.
- Si no existe acción de crear evento (ni estructurada ni Quick Add que cumpla), informar y detener.
- No inventar IDs de calendario. Si el schema requiere uno, resolverlo mediante el enum dinámico.
- Zona horaria: America/Santiago.
- La agenda debe reflejar aprendizaje progresivo y por etapas, no un volcado de temas.
- No pedir APIs nuevas, OAuth ni conexiones. Solo MCP Zapier ya autorizado.
- No incluir secretos, tokens ni datos no proporcionados.
- No commitear, pushear ni publicar nada como parte de esta skill.
