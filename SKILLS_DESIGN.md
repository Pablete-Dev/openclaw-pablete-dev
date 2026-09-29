# SKILLS_DESIGN.md

## Criterios generales de diseño

Estas skills se diseñan para Amato, no como plantillas genéricas. Usan **solo herramientas ya conectadas** (Gmail, GitHub, Telegram). **No requieren APIs ni OAuth nuevos**: si Gmail o GitHub ya resuelven la tarea, no se pide otra integración.

El comportamiento debe alinearse con `AGENTS.md`: inspección y preparación sin publicar; confirmación antes de enviar, publicar o hacer visible algo fuera de la máquina. `USER.md`, `SOUL.md` y `TOOLS.md` definen tono, cómo se llama a Pablete, zona horaria, estilo de correo y límites de GitHub/Telegram. El output tiene que **verse** como Amato: técnico, claro, directo, didáctico, sin relleno.

Acciones externas (enviar un correo, escribir a un canal que no sea el chat directo autorizado) **quedan pendientes de confirmación**. Los inputs de prueba serán **reales** (un destinatario, un repo, un periodo concretos), no `foo@example.com` ni `owner/repo` de relleno.

---

# Skill 1: Gmail Draft con la voz de Pablete

## Razón de diseño

**Por qué esta tarea.** Pablete redacta a menudo correos técnicos, coordinaciones, respuestas a clientes y solicitudes. El cuello de botella no es “saber escribir”, sino partir de un borrador con el tono correcto y los puntos obligatorios, listo para revisar en Gmail. Enviar no es el trabajo de la skill: el trabajo es **preparar**.

**Por qué ese input.** Un correo útil no se inventa. Hace falta destinatario (o tipo: cliente, proveedor, equipo), objetivo, contexto y lo que no puede faltar. El asunto es opcional porque a veces Pablete ya lo tiene y a veces Amato debe proponerlo. Sin esos datos, o se alucina o se pregunta de más; con ellos, se redacta y se deja en Gmail.

**Por qué ese formato de output.** Asunto + cuerpo + estado “no enviado / falta confirmación” coinciden con `TOOLS.md` (leer y borrador sí; enviar solo con OK) y con `AGENTS.md` (acciones externas se confirman). El evaluador puede ver de un vistazo: texto usable, voz coherente, y que **no** se disparó el envío.

## 1. ¿Qué hace esta skill?

Arma un **borrador de Gmail** con la voz de Pablete: profesional, cordial, directo y claro. Si el destinatario es cliente, baja tecnicismos; si es contexto técnico entre pares, puede ser más preciso sin volverse corporativo. Usa el contexto que entrega Pablete. **No envía.** Deja el borrador en Gmail para revisión. Cualquier envío posterior exige confirmación explícita.

## 2. ¿Qué input necesita el agente?

- Destinatario o tipo de destinatario (cliente, coordinación interna, solicitud, etc.).
- Objetivo del correo (informar, pedir, responder, coordinar).
- Contexto (qué pasó, qué se acordó, restricciones).
- Puntos obligatorios que deben aparecer.
- Opcional: asunto deseado.

Sin destinatario/tipo y sin objetivo, no se ejecuta la redacción a ciegas: se pide lo mínimo que falte.

## 3. ¿Cómo es un buen output?

- Asunto concreto.
- Cuerpo listo para leer, con tono alineado a `USER.md` / `SOUL.md` / `TOOLS.md`.
- Borrador creado o actualizado en Gmail, **sin enviar**.
- Indicación explícita de que **queda pendiente de confirmación** antes de cualquier envío.
- Sin copiar secretos, tokens ni datos que no vinieron en el contexto.

---

# Skill 2: GitHub Briefing a Telegram

## Razón de diseño

**Por qué esta tarea.** Pablete mueve varios repositorios y proyectos. Revisar commits, issues y PRs en cada pantalla consume tiempo. Un briefing corto en Telegram concentra el estado técnico: qué cambió, qué queda, qué merece atención. Encaja con GitHub (lectura) y Telegram (canal directo), sin merge, push ni mensajes a terceros.

**Por qué ese input.** Sin repositorio no hay objeto. El periodo o la cantidad de actividad acota el ruido (últimas N horas, últimos N commits, desde una fecha). El foco opcional (desarrollo, errores, entregas, cambios recientes) evita un volcado genérico y prioriza lo que Pablete necesita esa vez.

**Por qué ese formato de output.** Lista breve, sin tablas: `AGENTS.md` pide formato simple en Telegram. Bloques fijos (resumen, cambios, pendientes, atención) hacen el mensaje escaneable. Envío **solo** al chat directo de Pablete, y solo cuando la skill se ejecuta con esa acción autorizada: no grupos, no canales, no terceros.

## 1. ¿Qué hace esta skill?

Lee actividad reciente de un repositorio GitHub (commits, issues abiertas o relevantes, PRs si hay, cambios notables) y arma un **briefing breve** para Pablete. Puede señalar pendientes o riesgos **solo** a partir de lo que la API o la vista del repo muestran (issues abiertas, PRs sin merge, CI fallida visible, etc.). No inventa estado. **No** hace commit, push, merge ni cierra issues. El mensaje, cuando se envía, va al canal directo autorizado.

## 2. ¿Qué input necesita el agente?

- Repositorio (el que Pablete indique; sin IDs inventados).
- Periodo o cantidad de actividad a revisar (por tiempo o por volumen).
- Foco opcional: desarrollo, errores, entregas, cambios recientes, u otro acotado por Pablete.

## 3. ¿Cómo es un buen output?

- Resumen breve del estado.
- Cambios principales.
- Pendientes.
- Elementos que requieren atención (con base en datos, no en suposiciones).
- Formato simple para Telegram: listas, sin tablas, sin encabezados pesados.
- Entregado al chat directo de Pablete **únicamente** si la skill se ejecutó y el envío está autorizado; si no, el mismo contenido como texto para revisar, sin mandarlo a otro destino.
- Sin secretos de `.env`, tokens ni URLs internas sensibles extraídas de commits.

## Relación problema → input → comportamiento → resultado

| Skill | Problema | Input que lo acota | Comportamiento | Resultado verificable |
| --- | --- | --- | --- | --- |
| Gmail Draft | Correos frecuentes con tono y puntos fijos | Destinatario, objetivo, contexto, puntos, asunto opcional | Redactar y crear borrador; no enviar | Borrador en Gmail + aviso de confirmación |
| GitHub Briefing | Varios repos, poco tiempo para “estar al día” | Repo, ventana de actividad, foco | Solo lectura GitHub; Telegram solo al chat directo autorizado | Briefing escaneable, sin tablas, sin mutar el repo |
