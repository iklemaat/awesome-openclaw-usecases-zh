# Calendario Familiar y Asistente de Tareas Domésticas

Las familias modernas necesitan gestionar cinco o más calendarios simultáneamente — trabajo, personal, familiar compartido, escuela de niños, actividades extraescolares — distribuidos en diferentes plataformas y formatos. Los eventos importantes se pierden porque no hay una vista unificada. Mientras tanto, la coordinación de tareas domésticas (lista de compras, inventario de despensa, programación de citas) se realiza mediante mensajes de texto fragmentados, fácilmente se pierden.

Este caso de uso convierte OpenClaw en un coordinador familiar siempre activo: agrega calendarios para generar resumen matutino, monitorea mensajes para elementos accionables, y gestiona logística doméstica a través de una interfaz de chat compartida.

## Dolor

- **Fragmentación de calendarios**: El calendario de trabajo tiene restricciones de seguridad, no se puede compartir. El calendario de la escuela llega en PDF o páginas web escritas a mano. Los horarios de campamento de verano están en correos. Revisar manualmente cada calendario cada mañana no es sostenible — y "copiar eventos cross-calendros funciona, hasta que lo olvido y algo se pierde."
- **Sobrecarga de coordinación doméstica**: "¿Cuánta leche queda en casa?" requiere verificar el refrigerador personalmente, luego verificar la despensa del sótano, luego responder mensajes de texto. Esto se repite múltiples veces cada semana de compras.
- **Citas perdidas**: Las confirmaciones de citas llegan por mensaje de texto, y luego se quedan ahí sin que nadie las procese — sin evento de calendario, sin tiempo de desplazamiento buffer, sin recordatorio.

## Qué puede hacer

- **Resumen matutino**: Agregar todos los calendarios familiares en un resumen diario, enviar a través de tu canal preferido
- **Monitoreo ambiental de mensajes**: Vigilar conversaciones de iMessage/SMS, crear automáticamente eventos de calendario cuando detecta citas (confirmación de dentista, planes de reunión, etc.)
- **Buffer de tiempo de viaje**: Agregar segmentos de tiempo de tránsito antes y después de citas detectadas
- **Gestión de inventario familiar**: Mantener lista de inventario en tiempo real de artículos de despensa/refrigerador, cualquiera de las partes puede consultar en cualquier momento
- **Coordinación de compras**: Duplicar ingredientes cross-recetas, rastrear artículos con inventario bajo, generar lista de compras
- **Entrada de fotos**: Tomar foto de calendario escolar o contenido del congelador, el agente lo procesa como datos estructurados

## Habilidades requeridas

- Acceso a API de calendario (Google Calendar, Apple Calendar vía `ical`)
- Skill `imessage` para monitoreo de mensajes (solo macOS)
- Telegram o Slack para interfaz de chat familiar compartida
- Acceso a sistema de archivos para seguimiento de inventario
- Cámara/procesamiento de fotos para OCR de calendarios físicos

## Cómo configurar

### 1. Agregación de calendarios

Configurar OpenClaw para extraer datos de todas las fuentes de calendarios familiares:

El siguiente prompt hace que el agente agregue todas las fuentes de calendarios cada mañana y genere un resumen:

```text
## Calendar Sources

On morning briefing (8:00 AM):

1. Fetch my Google Work Calendar (read-only OAuth)
2. Fetch shared Family Google Calendar
3. Fetch partner's calendar (shared view)
4. Check ~/Documents/school-calendars/ for any new PDFs → OCR and extract events
5. Check recent emails for calendar attachments or event invitations

Compile into a single briefing:
- Today's events (all calendars, color-coded by source)
- Upcoming 3-day lookahead for conflicts
- Any new events added since yesterday
- Weather context for outdoor events

Deliver via Telegram/Slack family channel.
```

### 2. Monitoreo ambiental de mensajes

Esta es la diferenciación clave — el agente vigila pasivamente en segundo plano, toma acción cuando identifica contenido accionable:

El siguiente prompt configura al agente para revisar mensajes periódicamente y crear automáticamente eventos de calendario:

```text
## Message Monitoring (HEARTBEAT.md)

Every 15 minutes:
1. Check new iMessages across all conversations
2. Detect appointment-like patterns:
   - "Your appointment is confirmed for..."
   - "Can we meet on [date] at [time]?"
   - "Practice moved to Saturday at 3pm"
3. When detected:
   - Create calendar event with details
   - Add 30-minute driving buffer before AND after
   - Send confirmation to family Telegram: "Created: Dentist appointment, Tue 2pm. Added drive time 1:30-2:00 and 3:00-3:30."
   - If relevant to partner, add invite
4. Detect promise/commitment patterns:
   - "I'll send that over by Friday"
   - "Let's do dinner next week"
   → Create calendar hold or reminder
```

### 3. Gestión de inventario familiar

El siguiente prompt configura al agente para gestionar inventario familiar, soporta múltiples métodos de entrada como fotos, texto y recibos:

```text
## Pantry Tracking

Maintain ~/household/inventory.json with:
- Item name, quantity, location (fridge/pantry/basement)
- Last updated timestamp
- Low-stock threshold

Update methods:
- Photo: User sends photo of fridge/pantry → vision model extracts items
- Text: "We're out of eggs" / "Bought 2 gallons of milk"
- Receipt: Photo of grocery receipt → update inventory

Query: Either partner can ask via Telegram:
- "Do we have butter?" → Check inventory, respond with location and quantity
- "What's running low?" → List items below threshold
- "Generate grocery list" → Compile low-stock items + any recipe ingredients needed
```

## Ideas clave

- **Pasivo es mejor que activo**: El mayor avance es que el agente actúa sin que se lo pidan. Detectar citas en SMS y crear automáticamente eventos de calendario con buffer de viaje — "No le pedí que hiciera esto. Simplemente supo que era lo que quería."
- **Mac Mini es el mejor vehículo**: Este caso de uso es especialmente adecuado para ejecutar en un Mac Mini en casa — integración con iMessage, Apple Calendar y disponibilidad siempre activa
- **Comenzar con solo lectura**: Primero comenzar con lectura de calendarios y monitoreo de mensajes, luego habilitar operaciones de escritura (crear eventos, enviar mensajes)
- **Canal compartido de Telegram**: Hacer que ambas partes puedan ver qué está haciendo el agente — construye confianza y detecta errores temprano
- **Entrada de fotos está subestimada**: Tomar una foto de un PDF de calendario escolar o contenido del congelador es mucho más rápido que escribir — y los modelos visuales lo manejan muy bien

## Fuentes de inspiración

Este caso de uso combina múltiples patrones de la comunidad:

- **Agregación de calendarios**: Descrito por usuario de HN `angiolillo` en una [discusión de Hacker News](https://news.ycombinator.com/item?id=46872465), detalló el dolor de revisar cada mañana los calendarios de trabajo, personal, familiar y escuela de niños por separado.
- **Monitoreo ambiental de mensajes**: Documentado por [Sparkry AI](https://sparkryai.substack.com/p/24-hours-with-openclaw-the-ai-setup) — cuando la esposa recibió un mensaje de texto de cita dental, OpenClaw creó automáticamente un evento de calendario con buffer de viaje de 30 minutos, sin que se lo pidieran. Verificado también en [OpenClaw Showcase](https://openclaw.ai/showcase), donde `@theaaron` llamó a la gestión de calendarios basada en chat "uno de los mejores usos de LLM que he experimentado".
- **Coordinación doméstica**: El "Clawdbot Linguini" de Brandon Wang se ejecuta en un Mac Mini doméstico — maneja seguimientos de SMS, crea eventos de calendario desde fotos, rastrea precios de Airbnb, procesa fotos de inventario de congelador, y coordina logística doméstica vía iMessage y Slack.
- **Seguimiento de despensa**: Múltiples usuarios de HN discutieron el valor (y desafíos) de mantener inventario familiar, `dns_snek` señaló: "Olvido dónde puse las cosas 5 segundos después... Este es realmente un problema grande para mí, porque dejo que las cosas expiren."

## Enlaces relacionados

- [Skill de iMessage de OpenClaw](https://github.com/openclaw/openclaw)
- [API de Google Calendar](https://developers.google.com/calendar)
- [Apple Calendar (EventKit)](https://developer.apple.com/documentation/eventkit)
- [OpenClaw Showcase — Evaluaciones relacionadas con calendarios](https://openclaw.ai/showcase)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/family-calendar-household-assistant.md)
