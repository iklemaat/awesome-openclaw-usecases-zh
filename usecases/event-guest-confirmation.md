# Confirmación de Invitados a Eventos

Estás organizando un evento — cena, boda, evento corporativo — y necesitas confirmar asistencia de una lista de invitados. Llamar manualmente a 20+ personas es muy tedioso: tienes que llamar repetidamente, olvidar quién dijo qué, y perder información de restricciones dietéticas o invitados adicionales. Los mensajes de texto a veces funcionan, pero la gente ignora los mensajes. Las llamadas telefónicas reales obtienen mayor tasa de respuesta.

Este caso de uso hace que OpenClaw use el plugin [SuperCall](https://clawhub.ai/xonder/supercall) para llamar uno por uno a cada invitado de tu lista, confirmar si asisten, recopilar información de notas, y compilar todo en un resumen.

## Qué puede hacer

- Recorrer lista de invitados (nombre + número de teléfono) y llamar a cada uno
- IA se presenta amablemente como tu coordinador de eventos con un rol
- Confirmar fecha, hora y ubicación del evento con los invitados
- Preguntar si asisten, y recopilar notas (restricciones dietéticas, invitados adicionales, hora de llegada, etc.)
- Después de completar todas las llamadas, compilar resumen: quién confirmó, quién rechazó, quién no contestó, y todas las notas

## Por qué SuperCall

Este caso de uso usa específicamente el plugin [SuperCall](https://clawhub.ai/xonder/supercall) — no el plugin `voice_call` integrado. La diferencia clave es: SuperCall es un agente de voz completamente independiente. El rol de IA durante la llamada **solo tiene acceso al contexto que proporcionas** (nombre del rol, objetivo y saludo inicial). No puede acceder a tu agente de gateway, tus archivos, tus otras herramientas ni nada más.

Esto es importante para confirmación de invitados porque:

- **Seguridad**: La persona al otro lado del teléfono no puede manipular o acceder a tu agente mediante conversación. No hay riesgo de inyección de prompt (prompt injection) o fuga de datos.
- **Mejor conversación**: Como la IA está limitada a una tarea única enfocada (confirmar asistencia), puede mantener el tema y manejar la llamada más naturalmente que un agente de voz genérico.
- **Adecuado para procesamiento por lotes**: Vas a hacer muchas llamadas a diferentes personas. Un rol en caja de arena que se reinicia con cada llamada es exactamente lo que necesitas — las conversaciones no se afectan entre sí.

## Habilidades requeridas

- [SuperCall](https://clawhub.ai/xonder/supercall) — Instalar vía `openclaw plugins install @xonder/supercall`
- Una cuenta de Twilio con número de teléfono (para hacer llamadas salientes)
- Una clave de API de OpenAI (para IA de voz en tiempo real GPT-4o)
- ngrok (para túnel de webhook — versión gratis funciona)

Consulta la [documentación de SuperCall](https://github.com/xonder/supercall) para instrucciones completas de configuración.

## Cómo configurar

1. Instalar y configurar SuperCall según la [guía de configuración](https://github.com/xonder/supercall#configuration). Asegurar que hooks estén habilitados en tu configuración de OpenClaw.

2. Preparar tu lista de invitados. Puedes pegar directamente en el chat o guardar en un archivo:

```text
Lista de Invitados — BBQ de Verano, Sábado 14 de Junio, 4 PM, 23 Oak Street

- Sarah Johnson: +15551234567
- Mike Chen: +15559876543
- Rachel Torres: +15555551234
- David Kim: +15558887777
```

3. Configurar prompt para OpenClaw:

El siguiente prompt hace que el agente llame a los invitados uno por uno y compile las confirmaciones:

```text
I need you to confirm attendance for my event. Here are the details:

Event: Summer BBQ
Date: Saturday, June 14th at 4 PM
Location: 23 Oak Street

Here is my guest list:
<paste your guest list here>

For each guest, use supercall to call them. Use the persona "Jamie, event coordinator
for [your name]". The goal for each call is to confirm whether they're attending,
and note any dietary restrictions, plus-ones, or other comments.

After each call, log the result. Once all calls are done, give me a full summary:
- Who confirmed
- Who declined
- Who didn't answer
- Any notes or special requests from each guest
```

4. OpenClaw usará SuperCall para llamar a cada invitado uno por uno, luego resumirá los resultados. Puedes preguntar actualizaciones de progreso en cualquier momento.

## Ideas clave

- **Comenzar con prueba pequeña**: Primero probar con 2-3 invitados, asegurar que el rol y el saludo suenen bien. Puedes ajustar el tono antes de llamar a la lista completa.
- **Notar hora de llamadas**: No programar llamadas demasiado temprano o demasiado tarde. Puedes decirle a OpenClaw que solo llame durante ventanas de tiempo específicas.
- **Revisar registros de llamadas**: SuperCall guarda registros de llamadas en `~/clawd/supercall-logs`. Revisar después de las primeras llamadas para ver cómo van las conversaciones.
- **Manejo de llamadas no contestadas**: Si alguien no contesta, OpenClaw lo registrará, puedes decidir si reintentar más tarde o hacer seguimiento vía SMS.
- **Las llamadas telefónicas reales cuestan dinero**: Cada llamada consume minutos de Twilio. Configurar límites apropiados en tu cuenta de Twilio, especialmente para listas de invitados grandes.

## Enlaces relacionados

- [SuperCall en ClawHub](https://clawhub.ai/xonder/supercall)
- [SuperCall en GitHub](https://github.com/xonder/supercall)
- [Consola de Twilio](https://console.twilio.com)
- [API en tiempo real de OpenAI](https://platform.openai.com/docs/guides/realtime)
- [ngrok](https://ngrok.com)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/event-guest-confirmation.md)
