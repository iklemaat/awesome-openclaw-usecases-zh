# Sistema de Gestión de Estado de Proyectos: Esquema Impulsado por Eventos para Reemplazar Kanban

Los tableros Kanban tradicionales son estáticos y requieren actualizaciones manuales. Olvidas mover tarjetas, pierdes contexto entre sesiones, y no puedes rastrear el "por qué" detrás de los cambios de estado. Los proyectos se descarrilan sin visibilidad clara.

Este flujo de trabajo reemplaza los tableros Kanban con un sistema impulsado por eventos (event-driven), rastrea automáticamente el estado del proyecto:

- Almacena el estado del proyecto en una base de datos, mantiene historial completo
- Captura contexto: decisiones, elementos bloqueados, próximos pasos, ideas clave
- Actualización impulsada por eventos: "Acabo de completar X, bloqueado por Y" -> conversión automática de estado
- Consultas en lenguaje natural: "¿Cuál es el estado de [proyecto]?", "¿Por qué cambiamos de dirección en [función]?"
- Resumen de reunión diaria de pie: qué se hizo ayer, qué está planeado para hoy, qué está bloqueado
- Integración con Git: vincula commits a eventos del proyecto,实现 trazabilidad

## Dolor

Los tableros Kanban se vuelven obsoletos fácilmente. Gastas tiempo actualizando tarjetas en lugar de hacer trabajo real. El contexto se pierde — después de tres meses, ya no recuerdas por qué tomaste una decisión clave. No hay asociación automática entre cambios de código y progreso del proyecto.

## Qué puede hacer

En lugar de arrastrar tarjetas, simplemente hablas con tu asistente: "Completé el flujo de autenticación, comenzando el dashboard." El sistema registra eventos, actualiza el estado del proyecto y retiene contexto. Cuando preguntas "¿Cómo va el dashboard?" te da la historia completa: qué está completado, cuáles son los próximos pasos, qué te está bloqueando, y por qué.

Los commits de Git se escanean automáticamente y vinculan a proyectos. Tu resumen de reunión diaria de pie se genera automáticamente.

## Habilidades requeridas

- `postgres` o SQLite para base de datos de estado de proyectos
- `github` (CLI gh) para seguimiento de commits
- Discord o Telegram para actualizaciones y consultas
- Tareas programadas (cron job) para resúmenes diarios
- Sub-agentes (sub-agent) para análisis paralelo de proyectos

## Cómo configurar

1. Configurar base de datos de estado de proyectos:
```sql
-- Tabla de proyectos
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name TEXT UNIQUE,
  status TEXT, -- por ejemplo "active" (en progreso), "blocked" (bloqueado), "completed" (completado)
  current_phase TEXT,
  last_update TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla de eventos
CREATE TABLE events (
  id SERIAL PRIMARY KEY,
  project_id INTEGER REFERENCES projects(id),
  event_type TEXT, -- por ejemplo "progress" (progreso), "blocker" (bloqueo), "decision" (decisión), "pivot" (cambio de dirección)
  description TEXT,
  context TEXT,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla de elementos bloqueados
CREATE TABLE blockers (
  id SERIAL PRIMARY KEY,
  project_id INTEGER REFERENCES projects(id),
  blocker_text TEXT,
  status TEXT DEFAULT 'open', -- "open" (no resuelto), "resolved" (resuelto)
  created_at TIMESTAMPTZ DEFAULT NOW(),
  resolved_at TIMESTAMPTZ
);
```

2. Crear un canal de Discord para actualizaciones de proyectos (por ejemplo #project-state).

3. Configurar prompt para OpenClaw:

El siguiente prompt le dice al agente cómo gestionar automáticamente el estado de proyectos según tu conversación:

```text
You are my project state manager. Instead of Kanban, I'll tell you what I'm working on conversationally.

When I say things like:
- "Finished [task]" → log a "progress" event, update project state
- "Blocked on [issue]" → create a blocker entry, update project status to "blocked"
- "Starting [new task]" → log a "progress" event, update current phase
- "Decided to [decision]" → log a "decision" event with full context
- "Pivoting to [new approach]" → log a "pivot" event with reasoning

When I ask:
- "What's the status of [project]?" → fetch latest events, blockers, and current phase
- "Why did we decide [X]?" → search events for decision context
- "What's blocking us?" → list all open blockers across projects

Every morning at 9 AM, run a cron job to:
1. Scan git commits from the past 24 hours (via gh CLI)
2. Link commits to projects based on branch names or commit messages
3. Post a daily standup summary to Discord #project-state:
   - What happened yesterday (events + commits)
   - What's planned today (based on current phase and recent conversations)
   - What's blocked (open blockers)

When I'm planning a sprint, spawn a sub-agent to analyze each project's state and suggest priorities.
```

4. Integrar en tu flujo de trabajo: simplemente habla naturalmente con tu asistente sobre en qué estás trabajando, el sistema capturará todo automáticamente.

## Enlaces relacionados

- [Patrón de Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Por qué Kanban no funciona para desarrolladores independientes](https://blog.nuclino.com/why-kanban-doesnt-work-for-me)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/project-state-management.md)
