# CRM Personal (Gestión de Relaciones con Clientes) y Descubrimiento Automático de Contactos

Es casi imposible registrar manualmente a las personas que has conocido, cuándo se reunieron y qué discutieron. Los seguimientos importantes se omiten inadvertidamente, y olvidas el contexto antes de reuniones importantes.

Este flujo de trabajo construye y mantiene automáticamente un CRM personal:

- Tarea programada diariamente (cron job) escanea correos y calendario, descubre nuevos contactos y registros de interacciones
- Almacena contactos en base de datos estructurada con contexto de relaciones
- Consultas en lenguaje natural: "¿Qué sé de [alguien]?", "¿Quién necesita seguimiento?", "¿Cuándo fue la última vez que interactué con [alguien]?"
- Briefing de preparación de reuniones diarias: Antes de las reuniones del día, investiga a participantes externos a través del CRM e historial de correos, y proporciona un briefing

## Habilidades requeridas

- CLI `gog` (para Gmail y Google Calendar)
- Base de datos personalizada de CRM (SQLite o esquema similar), o usar skill [crm-query](https://clawhub.ai) (si está disponible)
- Tema de Telegram para consultas de CRM

## Cómo configurar

1. Crear base de datos de CRM:
```sql
CREATE TABLE contacts (
  id INTEGER PRIMARY KEY,
  name TEXT,
  email TEXT,
  first_seen TEXT,
  last_contact TEXT,
  interaction_count INTEGER,
  notes TEXT
);
```
2. Configurar un tema llamado "personal-crm" en Telegram para consultas.
3. Indicar a OpenClaw:

El siguiente prompt configura descubrimiento automático de contactos y briefing diario de reuniones:

```text
Run a daily cron job at 6 AM to:
1. Scan my Gmail and Calendar for the past 24 hours
2. Extract new contacts and update existing ones
3. Log interactions (meetings, emails) with timestamps and context

Also, every morning at 7 AM:
1. Check my calendar for today's meetings
2. For each external attendee, search my CRM and email history
3. Deliver a briefing to Telegram with: who they are, when we last spoke, what we discussed, and any follow-up items

When I ask about a contact in the personal-crm topic, search the database and give me everything you know.
```

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/personal-crm.md)
