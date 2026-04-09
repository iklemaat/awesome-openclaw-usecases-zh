# Asistente Personal Multicanal

Cambiar entre múltiples aplicaciones para gestionar tareas, agendar citas, enviar mensajes y rastrear trabajo es agotador. Necesitas una interfaz unificada que conecte todas las herramientas.

Este flujo de trabajo integra todas las funciones en un solo asistente de IA:

- Telegram como interfaz de interacción principal, con enrutamiento por temas (diferentes temas para ideas de video, CRM, ingresos, configuración, etc.)
- Integración con Slack para colaboración en equipo (asignación de tareas, guardar en base de conocimientos, activar ideas de video)
- Google Workspace: Crear eventos de calendario, gestionar correos, subir a Drive vía chat
- Todoist para captura rápida de tareas
- Asana para gestión de proyectos
- Recordatorios automáticos: Día de basura, boletín semanal de la empresa, etc.

## Habilidades requeridas

- CLI `gog` (Google Workspace)
- Integración con Slack (tokens de bot + usuario)
- Token de API de Todoist o skill
- Token de API de Asana o skill
- Canal de Telegram configurado con múltiples temas

## Cómo configurar

1. Configurar temas de Telegram para diferentes escenarios:
   - `config` — Configuración y depuración del robot
   - `updates` — Estado y notificaciones
   - `video-ideas` — Pipeline de contenido
   - `personal-crm` — Gestión de contactos
   - `earnings` — Seguimiento financiero
   - `knowledge-base` — Importación y consultas de datos RAG (generación aumentada por recuperación)

2. Conectar todas las herramientas vía configuración de OpenClaw:
   - OAuth de Google (Gmail, Calendar, Drive)
   - Slack (tokens de app + usuario)
   - Token de API de Todoist
   - Token de API de Asana

3. Indicar a OpenClaw:

El siguiente prompt configura enrutamiento multicanal y recordatorios automáticos:

```text
You are my multi-channel assistant. Route requests based on context:

Telegram topics:
- "config" → system settings, debugging
- "updates" → daily summaries, reminders, calendar
- "video-ideas" → content pipeline and research
- "personal-crm" → contact queries and meeting prep
- "earnings" → financial tracking
- "knowledge-base" → save and search content

When I ask you to:
- "Add [task] to my todo" → use Todoist
- "Create a card for [topic]" → use Asana Video Pipeline project
- "Schedule [event]" → use gog calendar
- "Email [person] about [topic]" → draft email via gog gmail
- "Upload [file] to Drive" → use gog drive

Set up automated reminders:
- Monday 6 PM: "Trash day tomorrow"
- Friday 3 PM: "Time to write the weekly company update"
```

4. Probar cada integración una por una, luego probar interacciones cross-flujo de trabajo (por ejemplo, guardar enlaces de Slack a base de conocimientos, luego usarlos en tarjetas de investigación de video).

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/multi-channel-assistant.md)
