# SKILLS_DESIGN.md

## Criterios generales de diseño

Estas skills se diseñan para Amato y para las **conexiones verificadas ahora** en el MCP de Zapier: **solo Google Docs y Google Calendar**. Gmail **no** está habilitado y **no se habilitará**. No se configurará ninguna API, OAuth ni conexión nueva. Si una idea de skill exige Gmail, GitHub, Telegram u otra app no listada, no entra en este diseño.

El comportamiento sigue `AGENTS.md`: inspeccionar y preparar; confirmar antes de crear, modificar o cancelar eventos de Calendar; no sobrescribir un Doc existente sin confirmación (`TOOLS.md`). `USER.md` y `SOUL.md` definen a Pablete, el español claro y didáctico, la formación progresiva en AI Engineering y `America/Santiago`. El output tiene que **verse** como Amato: técnico, directo, sin relleno; si la audiencia es cliente, sin tecnicismos de más.

Los tests usarán **datos reales** (un tema de seguimiento real, una sesión de estudio real), no placeholders. **Al menos una skill producirá un resultado real y verificable** en un servicio conectado (un Google Doc creado, y —tras confirmación— un evento de Calendar).

---

# Skill 1: Technical Follow-up Doc

Nombre futuro: `technical-followup-doc`

## Razón de diseño

**Por qué esta tarea.** Pablete trabaja con implementaciones, reuniones técnicas, minutas, seguimientos e incidentes. El valor está en pasar de notas sueltas a un documento estructurado que se puede compartir y reabrir. Google Docs ya está en Zapier: es el destino correcto, no un correo ni un doc local huérfano.

**Por qué ese input.** Título o tema, contexto, estado, acuerdos, pendientes y próximos pasos son lo que una minuta útil necesita para no quedar en “se habló de varias cosas”. La audiencia es opcional porque el mismo seguimiento puede ser para Pablete, para un equipo o para un cliente, y eso cambia el lenguaje. La skill **ya conoce** a Pablete, el estilo y las reglas de privacidad: no hay que re-enseñárselos en cada llamada.

**Por qué ese formato de output.** Un Doc real, con secciones fijas y un título descriptivo, es comprobable (URL o ID del documento). Encaja con `TOOLS.md` (Docs para documentación, minutas, planes). No se pide Gmail ni otra app.

## 1. ¿Qué hace esta skill?

Convierte el contexto real de una reunión, implementación, incidente o seguimiento técnico de Pablete en un documento estructurado y lo **crea en Google Docs** con el Zapier MCP ya autorizado.

## 2. ¿Qué input necesita?

- Título o tema.
- Contexto (qué se hizo, qué se discutió, qué falló).
- Estado actual.
- Acuerdos o decisiones.
- Pendientes.
- Próximos pasos.
- Destinatario o audiencia opcional (Pablete, equipo, cliente).

Ya conocido, no se pide de nuevo: Pablete; estilo técnico, claro, directo y didáctico; para clientes, menos tecnicismos; privacidad y no filtrar secretos; no asumir acciones externas extra (no hay envío de mail).

Si faltan datos críticos para no inventar acuerdos o estados, se pregunta antes de crear el Doc.

## 3. ¿Cómo es un buen output?

- Un **Google Doc real**, creado (no un pegado solo en el chat).
- Título descriptivo.
- Resumen.
- Estado actual.
- Acuerdos.
- Pendientes.
- Próximos pasos.
- Lenguaje adaptado a la audiencia.
- Confirmación verificable de creación (enlace o identificador que Zapier/Docs devuelva).
- Sin sobrescribir un documento existente sin confirmación.
- Sin secretos, tokens ni datos que no vinieron en el contexto.

---

# Skill 2: AI Engineering Study Session

Nombre futuro: `ai-engineering-study-session`

## Razón de diseño

**Por qué esta tarea.** Pablete cursa AI Engineering y rinde mejor con sesiones cortas, por etapas y con un objetivo concreto. Calendar ya está conectado: sirve para bloquear tiempo, no para “recordatorios” en otra app.

**Por qué ese input.** Tema, objetivo, duración y fecha o ventana son lo mínimo para no crear un evento vacío o a una hora absurda. Notas y restricciones (exámenes, trabajo, “solo después de las 19:00”) evitan chocar con la vida real. La skill **ya conoce** la formación progresiva, `America/Santiago`, el estilo didáctico y el avance por etapas: la agenda de estudio debe ser incremental, no un temario de tres semestres en una hora.

**Por qué ese formato de output.** Primero una propuesta (fecha/hora, duración, objetivo, agenda breve) porque `TOOLS.md` y `AGENTS.md` exigen confirmación antes de crear, modificar o cancelar eventos. Después del sí, un evento **real** en Google Calendar, comprobable. Nunca se crea el evento en el mismo paso que la propuesta.

## 1. ¿Qué hace esta skill?

Prepara una sesión de estudio personalizada para Pablete (objetivo + agenda breve en su zona horaria) y, **solo después de confirmación explícita**, crea el evento en Google Calendar con la conexión existente.

## 2. ¿Qué input necesita?

- Tema a estudiar.
- Objetivo de la sesión.
- Duración.
- Fecha o ventana deseada.
- Notas o restricciones opcionales.

Ya conocido: formación progresiva en AI Engineering; zona horaria `America/Santiago`; estilo didáctico; preferencia por avanzar por etapas.

Sin tema y sin ventana o duración, no se propone un horario inventado: se pide lo que falte.

## 3. ¿Cómo es un buen output?

**Antes de crear**

- Propuesta de evento.
- Fecha y hora en `America/Santiago`.
- Duración.
- Objetivo.
- Agenda breve de estudio, por etapas, acorde al nivel progresivo.

**Después de confirmación explícita**

- Evento real en Google Calendar.
- Confirmación del evento creado (datos o enlace que Calendar/Zapier devuelva).

Nunca crear, modificar ni cancelar un evento sin esa confirmación. No usar Gmail ni otras apps. No inventar IDs de calendario.

## Relación problema → input → comportamiento → resultado

- **technical-followup-doc:** el problema es pasar de contexto técnico suelto a minuta usable. El input son tema, estado, acuerdos, pendientes y audiencia. El comportamiento es redactar con la voz de Pablete/Amato y **crear un Google Doc**. El resultado verificable es el documento en Docs.
- **ai-engineering-study-session:** el problema es estudiar AI Engineering con bloques concretos. El input son tema, objetivo, duración y ventana. El comportamiento es proponer y **esperar confirmación**, luego crear en Calendar. El resultado verificable es la propuesta primero y el evento después del OK.
