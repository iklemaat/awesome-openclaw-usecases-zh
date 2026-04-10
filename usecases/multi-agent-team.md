# Equipo Profesional Multi-Agente (Configuración para Fundadores Independientes)

Los fundadores independientes juegan múltiples roles — estrategia, desarrollo, marketing, ventas, operaciones. Cambiar frecuentemente entre estos roles destruye el trabajo profundo. Contratar es caro y lento. ¿Y si pudieras iniciar un pequeño equipo de agentes de AI profesionales, cada uno con un rol y personalidad únicos, todo controlado a través de una única interfaz de chat?

Este caso de uso configura múltiples agentes de OpenClaw como un equipo coordinado, cada agente se enfoca en un dominio, se comunica a través de memoria compartida, y se controla vía Telegram.

## Dolor

- **Un solo agente no puede hacer todo**: Cuando manejas estrategia, código, investigación de marketing y análisis de negocios simultáneamente, la ventana de contexto de un solo agente se llena rápidamente
- **Falta de especialización**: Los prompts genéricos producen resultados genéricos — los agentes de codificación no deberían escribir copy de marketing simultáneamente
- **Agotamiento del fundador independiente**: Necesitas un equipo, no otra herramienta que gestionar. Los agentes deberían trabajar en segundo plano y presentar resultados, no requerir supervisión continua
- **Islas de conocimiento**: Las ideas de investigación de marketing no afectan automáticamente las prioridades de desarrollo, a menos que las puentes manualmente

## Qué puede hacer

- **Agentes especializados**: Cada agente tiene un rol único, personalidad y modelo optimizado para su dominio
- **Memoria compartida**: Documentos de proyecto, objetivos y decisiones clave son accesibles para todos los agentes — nada se pierde
- **Contexto privado**: Cada agente también mantiene su propio historial de conversaciones y notas específicas de dominio
- **Panel de control unificado**: Todos los agentes se acceden a través de un solo grupo de chat de Telegram — @mencionas el agente que necesitas
- **Tareas programadas diarias**: Los agentes trabajan proactivamente sin que se lo pidan — sugerencias de contenido, monitoreo de competencia, seguimiento de métricas
- **Ejecución paralela**: Múltiples agentes pueden manejar tareas independientes simultáneamente

## Configuración de equipo de ejemplo

### Agente 1: Milo (Líder de estrategia)

```text
## SOUL.md — Milo

You are Milo, the team lead. Confident, big-picture, charismatic.

Responsibilities:
- Strategic planning and prioritization
- Coordinating the other agents
- Weekly goal setting and OKR tracking
- Synthesizing insights from all agents into actionable decisions

Model: Claude Opus
Channel: Telegram (responds to @milo)

Daily tasks:
- 8:00 AM: Review overnight agent activity, post morning standup summary
- 6:00 PM: End-of-day recap with progress toward weekly goals
```

### Agente 2: Josh (Negocios y crecimiento)

```text
## SOUL.md — Josh

You are Josh, the business analyst. Pragmatic, straight to the point, numbers-driven.

Responsibilities:
- Pricing strategy and competitive analysis
- Growth metrics and KPI tracking
- Revenue modeling and unit economics
- Customer feedback analysis

Model: Claude Sonnet (fast, analytical)
Channel: Telegram (responds to @josh)

Daily tasks:
- 9:00 AM: Pull and summarize key metrics
- Track competitor pricing changes weekly
```

### Agente 3: Agente de marketing

```text
## SOUL.md — Marketing Agent

You are the marketing researcher. Creative, curious, trend-aware.

Responsibilities:
- Content ideation and drafting
- Competitor social media monitoring
- Reddit/HN/X trend tracking for relevant topics
- SEO keyword research

Model: Gemini (strong at web research and long-context analysis)
Channel: Telegram (responds to @marketing)

Daily tasks:
- 10:00 AM: Surface 3 content ideas based on trending topics
- Monitor competitor Reddit/X mentions daily
- Weekly content calendar draft
```

### Agente 4: Agente de desarrollo

```text
## SOUL.md — Dev Agent

You are the dev agent. Precise, thorough, security-conscious.

Responsibilities:
- Coding and architecture decisions
- Code review and quality checks
- Bug investigation and fixing
- Technical documentation

Model: Claude Opus / Codex (for implementation)
Channel: Telegram (responds to @dev)

Daily tasks:
- Check CI/CD pipeline health
- Review open PRs
- Flag technical debt items
```

## Habilidades requeridas

- Skill `telegram` para interfaz de control compartida
- `sessions_spawn` / `sessions_send` para coordinación multi-agente
- Sistema de archivos compartido o herramienta de notas para memoria de equipo
- Claves de API independientes de diferentes proveedores de modelos (si usas modelos híbridos)
- Una VPS o máquina siempre activa para ejecutar los agentes

## Cómo configurar

### 1. Estructura de memoria compartida

```text
team/
├── GOALS.md           # OKR y prioridades actuales (legible por todos los agentes)
├── DECISIONS.md       # Registro de decisiones clave (solo agregar)
├── PROJECT_STATUS.md  # Estado actual de proyectos (todos los agentes actualizan)
├── agents/
│   ├── milo/          # Contexto y notas privadas de Milo
│   ├── josh/          # Contexto privado de Josh
│   ├── marketing/     # Material de investigación del agente de marketing
│   └── dev/           # Notas técnicas del agente de desarrollo
```

### 2. Enrutamiento de Telegram

Configurar un grupo de Telegram que todos los agentes escuchan, pero cada agente solo responde cuando es @mencionado:

El siguiente prompt configura reglas de enrutamiento de agentes en el grupo de Telegram:

```text
## AGENTS.md — Telegram Routing

Telegram group: "Team"

Routing:
- @milo → Strategy agent (spawns/resumes milo session)
- @josh → Business agent (spawns/resumes josh session)
- @marketing → Marketing agent (spawns/resumes marketing session)
- @dev → Dev agent (spawns/resumes dev session)
- @all → Broadcast to all agents
- No tag → Milo (team lead) handles by default

Each agent:
1. Reads shared GOALS.md and PROJECT_STATUS.md for context
2. Reads its own private notes
3. Processes the message
4. Responds in Telegram
5. Updates shared files if the response involves a decision or status change
```

### 3. Tareas programadas

El siguiente prompt configura tareas automatizadas diarias y semanales del equipo:

```text
## HEARTBEAT.md — Team Schedule

Daily:
- 8:00 AM: Milo posts morning standup (aggregates overnight agent activity)
- 9:00 AM: Josh pulls key metrics
- 10:00 AM: Marketing surfaces content ideas from trending topics
- 6:00 PM: Milo posts end-of-day recap

Ongoing:
- Dev: Monitor CI/CD health, review PRs as they come in
- Marketing: Reddit/X keyword monitoring (every 2 hours)
- Josh: Competitor pricing checks (weekly)

Weekly:
- Monday: Milo drafts weekly priorities (input from all agents)
- Friday: Josh compiles weekly metrics report
```

## Ideas clave

- **La personalidad es más importante de lo que crees**: Dar a los agentes nombres únicos y estilos de comunicación hace que "conversar con tu equipo" sea natural, en lugar de luchar con una AI genérica
- **Memoria compartida + contexto privado**: Esta combinación es crucial — los agentes necesitan una base común (objetivos, decisiones), pero también necesitan su propio espacio para acumular experiencia de dominio
- **Elegir el modelo correcto para el trabajo correcto**: No uses un modelo de razonamiento costoso para monitoreo de palabras clave. Empareja capacidades del modelo con complejidad de tareas
- **Las tareas programadas son el volante**: El verdadero valor está en que los agentes presenten insights proactivamente, no en esperar a que preguntes
- **Comienza con 2, no con 4**: Configura un líder más un experto primero, luego agrega agentes cuando descubras cuellos de botella

## Fuentes de inspiración

Este patrón fue descrito por [Trebuh en X](https://x.com/iamtrebuh/status/2011260468975771862), un fundador independiente que configuró 4 agentes de OpenClaw — Milo (líder de estrategia), Josh (negocios), agente de marketing y agente de desarrollo — todos controlados vía un solo chat de Telegram en una VPS. Cada agente tiene su propia personalidad, modelo y tareas programadas, mientras comparte memoria de proyectos. Lo describió como "un verdadero pequeño equipo disponible 24/7".

Este patrón también fue verificado en [OpenClaw Showcase](https://openclaw.ai/showcase), `@jdrhyne` reportó ejecutar "15+ agentes, 3 máquinas, 1 servidor de Discord — básicamente todo IT se construye vía chat", `@nateliason` describió una pipeline multi-modelo (prototipo -> resumen -> optimización -> implementación -> repetir) usando diferentes modelos en cada etapa. Otro usuario `@danpeguine` ejecuta dos instancias diferentes de OpenClaw en el mismo grupo de WhatsApp para colaboración.

## Enlaces relacionados

- [Documentación de sub-agentes de OpenClaw](https://github.com/openclaw/openclaw)
- [Skill de Telegram de OpenClaw](https://github.com/openclaw/openclaw)
- [OpenClaw Showcase](https://openclaw.ai/showcase)
- [Anthropic: Construcción de agentes eficientes](https://www.anthropic.com/research/building-effective-agents)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/multi-agent-team.md)
