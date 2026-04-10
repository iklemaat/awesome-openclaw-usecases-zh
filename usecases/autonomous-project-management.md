# Gestión Autónoma de Proyectos con Sub-Agentes

Gestionar proyectos complejos con múltiples flujos de trabajo paralelos es agotador. Tienes que cambiar de contexto frecuentemente, rastrear estados en diferentes herramientas, y coordinar manualmente las transiciones de tareas.

Este caso de uso implementa un patrón de gestión de proyectos descentralizado, donde sub-agentes (sub-agent) manejan tareas autónomamente, coordinándose a través de archivos de estado compartidos en lugar de un planificador central.

## Dolor

El patrón de planificador tradicional crea cuellos de botella — el agente principal se convierte en un "policía de tráfico". Para proyectos complejos (refactorización de múltiples repositorios, sprints de investigación, pipelines de contenido), necesitas agentes que puedan trabajar en paralelo sin supervisión continua.

## Descripción general de funciones

- **Coordinación descentralizada**: Los agentes leen y escriben en un archivo `STATE.yaml` compartido para coordinarse
- **Ejecución paralela**: Múltiples sub-agentes manejan tareas independientes simultáneamente
- **Sin sobrecarga de planificador**: La sesión principal permanece精简 (modo CEO — solo estrategia)
- **Auto-documentación**: Todo el estado de tareas se persiste en archivos bajo control de versiones

## Patrón central: STATE.yaml

Cada proyecto mantiene un archivo `STATE.yaml` como única fuente de verdad:

```yaml
# STATE.yaml - Archivo de coordinación de proyectos
project: website-redesign
updated: 2026-02-10T14:30:00Z

tasks:
  - id: homepage-hero
    status: in_progress
    owner: pm-frontend
    started: 2026-02-10T12:00:00Z
    notes: "Working on responsive layout"

  - id: api-auth
    status: done
    owner: pm-backend
    completed: 2026-02-10T14:00:00Z
    output: "src/api/auth.ts"

  - id: content-migration
    status: blocked
    owner: pm-content
    blocked_by: api-auth
    notes: "Waiting for new endpoint schema"

next_actions:
  - "pm-content: Resume migration now that api-auth is done"
  - "pm-frontend: Review hero with design team"
```

## Cómo funciona

1. **Agente principal recibe tarea** → Genera sub-agentes con alcance específico
2. **Sub-agente lee STATE.yaml** → Encuentra tareas asignadas a sí mismo
3. **Sub-agente trabaja autónomamente** → Actualiza progreso en STATE.yaml
4. **Otros agentes monitorean STATE.yaml** → Reclaman trabajo desbloqueado
5. **Agente principal verifica periódicamente** → Revisa estado, ajusta prioridades

## Habilidades requeridas

- `sessions_spawn` / `sessions_send` para gestión de sub-agentes
- Acceso a sistema de archivos para manipular STATE.yaml
- Git para versionado de estado (opcional pero recomendado)

## Configuración: AGENTS.md

```text
## PM 委派模式

主会话 = 仅作为协调者。所有执行交给子智能体。

工作流程：
1. 新任务到达
2. 检查 PROJECT_REGISTRY.md 查找现有 PM
3. 如果 PM 存在 → sessions_send(label="pm-xxx", message="[task]")
4. 如果是新项目 → sessions_spawn(label="pm-xxx", task="[task]")
5. PM 执行任务，更新 STATE.yaml，汇报结果
6. 主智能体向用户总结

规则：
- 主会话：最多 0-2 次工具调用（仅 spawn/send）
- PM 拥有自己的 STATE.yaml 文件
- PM 可以生成子子智能体处理并行子任务
- 所有状态变更提交到 git
```

## Ejemplo: Generar un PM

El siguiente es un ejemplo de solicitud de usuario y respuesta del agente:

```text
User: "Refactor the auth module and update the docs"

Main agent:
1. Checks registry → no active pm-auth
2. Spawns: sessions_spawn(
     label="pm-auth-refactor",
     task="Refactor auth module, update docs. Track in STATE.yaml"
   )
3. Responds: "Spawned pm-auth-refactor. I'll report back when done."

PM subagent:
1. Creates STATE.yaml with task breakdown
2. Works through tasks, updating status
3. Commits changes
4. Reports completion to main
```

## Ideas clave

- **STATE.yaml es mejor que un planificador**: La coordinación basada en archivos es más escalable que el paso de mensajes
- **Git como registro de auditoría**: Confirmar cambios de STATE.yaml para obtener historial completo
- **La convención de nombres de etiquetas importa**: Usa formato `pm-{project}-{scope}` para rastreo fácil
- **Sesión principal精简**: Cuanto menos hace el agente principal, más rápidas son las respuestas

## Fuentes de inspiración

Este patrón está inspirado en el [método de Nicholas Carlini](https://nicholas.carlini.com/) — dejar que los agentes se auto-organicen, en lugar de micro-gestionarlos.

## Enlaces relacionados

- [Documentación de sub-agentes de OpenClaw](https://github.com/openclaw/openclaw)
- [Anthropic: Construcción de agentes eficientes](https://www.anthropic.com/research/building-effective-agents)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/autonomous-project-management.md)
