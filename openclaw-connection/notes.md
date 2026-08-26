\# OpenClaw Connection



Se configuró OpenClaw en un VPS y se validó la conexión del bot de Telegram mediante `openclaw status --deep`.



Para integrar servicios externos se utilizó Zapier MCP mediante `mcporter`, habilitando Google Docs y Google Calendar.



Durante la configuración fue necesario completar la autenticación OAuth tanto para Zapier MCP como para OpenClaw.



Se validó Google Docs creando un documento de prueba desde Zapier MCP y comprobando su creación directamente en Google Docs.



Se validó Google Calendar creando un evento de 30 minutos y verificándolo posteriormente en el calendario.



Finalmente se realizó una prueba completa desde Telegram, donde OpenClaw creó un documento, solicitó la hora faltante para la reunión y posteriormente creó el evento mediante Zapier MCP.



El flujo final validado fue: Telegram → OpenClaw → Zapier MCP → Google Docs / Google Calendar → confirmación en Telegram.



