# Equipo de Desarrollo de Una Persona Agent Swarm (Guía de configuración completa)

> Incluye adaptación local: Notificaciones Feishu/DingTalk / Esquemas alternativos para Codex / Plataformas de monitoreo locales
>
> **Requiere mayor habilidad técnica**: Este caso de uso involucra Git Worktree, tmux, Cron, gh CLI y otras herramientas a nivel de sistema, se recomienda para usuarios con experiencia en línea de comandos Linux/macOS y base de CI/CD.

¿Codificar directamente con Codex o Claude Code? Solo ven el código, no ven el panorama completo de tu negocio. La ventana de contexto (context window) es un juego de suma cero — llenar con código no deja espacio para contexto de negocio, llenar con historial de clientes no deja espacio para código.

OpenClaw como capa de orquestación cambia esta ecuación: mantiene todo tu contexto de negocio (datos de clientes, actas de reuniones, decisiones históricas, experiencias de éxito y fracaso), y convierte este contexto en Prompts precisos distribuidos a varios Agentes de codificación. Los Agentes de codificación se enfocan en código, el Agente orquestador controla la estrategia — **profesionalización a través de contexto, no a través de modelos diferentes**.

## Qué puede hacer

- **Orquestación de modelos heterogéneos**: El orquestador ("Zoe") selecciona automáticamente Codex (modelo de codificación de OpenAI) o Claude Code (herramienta de codificación de Anthropic) según tipo de tarea, enruta al modelo más adecuado
- **Aislamiento con Git Worktree (árbol de trabajo)**: Cada Agente trabaja en Worktree y rama independientes, no interfieren entre sí
- **Gestión de sesiones con tmux (multiplexor de terminal)**: Agentes se ejecutan en sesiones tmux, soporta enviar instrucciones a mitad de camino para corregir, no necesita terminar y reiniciar
- **Revisión de código de tres modelos**: Cada PR pasa automáticamente por revisión triple de Codex, Gemini Code Assist, Claude Code, complementando puntos ciegos
- **Ciclo de monitoreo con Cron**: Cada 10 minutos detecta automáticamente estado de todos los Agentes (vivo en tmux, estado de PR, resultado de CI), fallo auto-renace (máximo 3 veces)
- **Prompt de auto-mejora (Ralph Loop V2)**: Al fallar el orquestador reescribe Prompt combinando contexto de negocio, no reintento simple; modo exitoso se sedimenta automáticamente como experiencia reutilizable
- **Descubrimiento activo de trabajo**: Escaneo matutino de errores en Sentry → auto-reparación, escaneo de notas después de reuniones → genera requisitos de funciones, registro de cambios y documentación vespertino
- **Notificar solo al completar**: Push a Telegram solo después de que PR pasa todos los checks (CI + revisión de tres modelos + capturas), evita ruido de información

## Habilidades requeridas

- Versión OpenClaw 2026.2+
- `sessions_spawn` / `sessions_send` (coordinación multi-Agente)
- Skill `telegram` (canal de notificación, reemplazable localmente por Feishu/DingTalk)
- `file_read` / `file_write` (gestión de registro de tareas)
- `cron` (monitoreo programado)
- Git + [gh CLI](https://cli.github.com/) (gestión de PR y verificación de estado de CI)
- [tmux](https://github.com/tmux/tmux) (gestión de sesiones de Agentes)
- CLI de Codex o CLI de Claude Code (Agentes de codificación)

## Arquitectura central

```
Tú (Tomador de decisiones)
 └── Zoe (Orquestador OpenClaw, contexto de negocio en Obsidian Vault)
       ├── Agente Codex 1 (feat/custom-templates, tmux: codex-templates)
       ├── Agente Codex 2 (fix/billing-bug, tmux: codex-billing)
       ├── Agente Claude Code (feat/ui-refresh, tmux: cc-ui)
       └── Script de monitoreo Cron (cada 10 minutos, check-agents.sh)
            ├── Verificar si sesiones tmux están vivas
            ├── Verificar PR y estado de CI vía gh CLI
            └── Fallo → Zoe reescribe Prompt → Renacer Agente
```

Separación de contexto de dos capas es el diseño central:

| | Orquestador OpenClaw (Zoe) | Agente de codificación (Codex / Claude Code) |
|---|---|---|
| **Contexto** | Datos de clientes, actas de reuniones, decisiones históricas, patrones de éxito/fracaso | Base de código, definiciones de tipos, archivos de prueba, especificaciones de API |
| **Responsabilidad** | Determinación de alcance, generación de Prompt, programación de Agentes, análisis de fallos | Codificación, pruebas, commit, creación de PR |
| **Permisos** | API Admin, base de datos de solo lectura, registro de tareas | Solo base de código y CI |

## Flujo de 8 pasos

### Paso 1: Requisito de cliente → Determinar alcance con orquestador

Después de que cliente plantea requisito, discutir con orquestador. Como las actas de reuniones se sincronizan automáticamente a Obsidian Vault, el orquestador ya tiene contexto completo, no necesita explicación adicional.

El orquestador completa tres cosas:
1. Recargar o desbloquear función para cliente vía API Admin (responde al cliente inmediatamente)
2. Extraer configuración existente del cliente a través de acceso de base de datos de solo lectura (Agentes de codificación nunca tienen permisos de base de datos de producción)
3. Generar Prompt con contexto completo y Spawn Agente de codificación

### Paso 2: Generar Agente

Cada Agente obtiene Worktree independiente y sesión tmux:

```bash
# Crear worktree + iniciar agente
git worktree add ../feat-custom-templates -b feat/custom-templates origin/main
cd ../feat-custom-templates && pnpm install

tmux new-session -d -s "codex-templates" \
  -c "/path/to/worktrees/feat-custom-templates" \
  "$HOME/.clawdbot/run-agent.sh templates gpt-5.3-codex high"
```

Comando para iniciar Agente de codificación:

```bash
# Codex
codex --model gpt-5.3-codex \
  -c "model_reasoning_effort=high" \
  --dangerously-bypass-approvals-and-sandbox \
  "Your prompt here"

# Claude Code
claude --model claude-opus-4.5 \
  --dangerously-skip-permissions \
  -p "Your prompt here"
```

Ventaja central de tmux — corrección a mitad de camino, no necesita terminar Agente:

```bash
# ¿Agente tomó dirección equivocada?
tmux send-keys -t codex-templates "Stop. Focus on the API layer first, not the UI." Enter

# ¿Agente necesita más contexto?
tmux send-keys -t codex-templates "The schema is in src/types/template.ts. Use that." Enter
```

Tareas se rastrean en `.clawdbot/active-tasks.json`:

```json
{
  "id": "feat-custom-templates",
  "tmuxSession": "codex-templates",
  "agent": "codex",
  "description": "Plantillas de correo personalizadas para cliente de agencia",
  "repo": "medialyst",
  "worktree": "feat-custom-templates",
  "branch": "feat/custom-templates",
  "startedAt": 1740268800000,
  "status": "running",
  "notifyOnComplete": true
}
```

### Paso 3: Ciclo de monitoreo Cron

Ejecutar `.clawdbot/check-agents.sh` cada 10 minutos, lógica 100% determinística, consumo extremadamente bajo de Tokens:

- Verificar si sesiones tmux están vivas
- Verificar si hay PR abierto vía CLI `gh`
- Verificar estado de CI
- Fallo de CI o comentarios críticos de revisión → Renacer Agente automáticamente (máximo 3 veces)
- Solo alertar cuando necesita intervención humana

No sondear Agentes directamente (sería caro), sino leer registro JSON y estado externo.

### Paso 4: Agente crea PR

Agente envía PR vía `gh pr create --fill`. En este momento **no te notifica** — creación de PR no es igual a completado.

Definición de Terminado (asegura que tu Agente sepa esto):

- ✅ PR creado
- ✅ Rama sincronizada con main (sin conflictos de merge)
- ✅ CI pasa (lint, verificación de tipos, pruebas unitarias, E2E)
- ✅ Revisión de Codex pasa
- ✅ Revisión de Claude Code pasa
- ✅ Revisión de Gemini pasa
- ✅ Cambios de UI incluyen capturas

Después de completar, registro de tareas se actualiza automáticamente:

```json
{
  "status": "done",
  "pr": 341,
  "completedAt": 1740275400000,
  "checks": {
    "prCreated": true,
    "ciPassed": true,
    "codexReviewPassed": true,
    "claudeReviewPassed": true,
    "geminiReviewPassed": true
  },
  "note": "Todos los checks pasaron. Listo para merge."
}
```

### Paso 5: Revisión automática de código de tres modelos

| Revisor | Fortaleza | Descripción |
|--------|------|------|
| Codex | Casos límite, errores lógicos, condiciones de carrera | Más profundo, tasa de falsos positivos extremadamente baja |
| Gemini Code Assist | Vulnerabilidades de seguridad, problemas de escalabilidad | Gratis, da sugerencias de reparación específicas |
| Claude Code | Verifica hallazgos de otros revisores | Tiende a ser excesivamente conservador, solo enfoca en nivel Crítico |

Los tres revisores comentan directamente en el PR.

### Paso 6: Pruebas automatizadas

Pipeline de CI incluye:

- Lint + verificación de tipos de TypeScript
- Pruebas unitarias
- Pruebas E2E
- Pruebas Playwright (comparado con entorno de preview consistente con producción)

Regla: PR con cambios de UI debe incluir capturas en descripción, de lo contrario CI falla directamente. Esto acorta significativamente tiempo de revisión — no necesita abrir entorno de preview manualmente.

### Paso 7: Revisión humana

En este momento recibes notificación: "PR #341 listo para revisión". CI ya pasó, tres revisores de IA aprobaron, capturas muestran cambios de UI. Revisión humana solo toma 5-10 minutos, muchos PR se pueden merge directamente después de ver capturas.

### Paso 8: Merge y limpieza

PR merge. Cron diario limpia Worktrees huérfanos y registro de tareas vencidas.

## Ralph Loop V2: Sistema de Prompt de auto-mejora

Ralph Loop tradicional usa el mismo Prompt en cada ciclo, la experiencia acumulada mejora calidad de recuperación, pero el Prompt en sí es estático.

Este sistema es diferente. Cuando Agente falla, el orquestador analiza la causa combinando **contexto de negocio** y reescribe Prompt:

- ¿Contexto de Agente desbordado? → "Focus only on these three files."
- ¿Agente tomó dirección equivocada? → "Stop. The customer wanted X, not Y. Here's what they said in the meeting."
- ¿Agente necesita aclaración? → "Here's the customer's email and what their company does."

El orquestador también descubre trabajo activamente:

- **Matutino**: Escanear Sentry → detectar 4 errores nuevos → generar 4 Agentes de reparación
- **Después de reuniones**: Escanear notas → marcar 3 solicitudes de funciones → generar 3 Agentes Codex
- **Vespertino**: Escanear log de Git → generar Claude Code para actualizar Changelog y documentación de cliente

Señal de recompensa: CI pasa + revisión de tres modelos pasa + merge humano. Cualquier fallo desencadena ciclo. Patrones exitosos se registran:

- "Esta estructura de Prompt funciona para funciones de facturación."
- "Codex necesita definiciones de tipos al inicio."
- "Siempre incluir rutas de archivos de prueba."

## Guía de selección de modelos

| Modelo | Escenario aplicable | Características |
|------|----------|------|
| Codex (gpt-5.3-codex) | Lógica backend, Bugs complejos, Refactorización multi-archivo | Lento pero profundo, usado para 90% de tareas |
| Claude Code (claude-opus-4.5) | Frontend, Operaciones Git | Rápido, menos problemas de permisos |
| Gemini | Especificaciones de diseño de UI | Primero usar Gemini para generar especificaciones HTML/CSS, luego entregar a Claude Code para implementar |

El orquestador enruta automáticamente según tipo de tarea: Bug de sistema de facturación → Codex, modificación de estilo de botón → Claude Code, diseño de nuevo dashboard → Gemini genera borrador + Claude Code implementa.

## Cómo configurar

Copiar el contenido de este artículo a OpenClaw, usar el siguiente prompt:

```text
Implement this agent swarm setup for my codebase:

1. Create .clawdbot/ directory with:
   - run-agent.sh: Script to spawn coding agents in tmux sessions with worktree isolation
   - check-agents.sh: Deterministic monitoring script that checks tmux sessions, PR status (via gh cli), and CI status
   - active-tasks.json: Task registry tracking all running agents

2. Set up cron job to run check-agents.sh every 10 minutes

3. Configure notification channel (Telegram/Feishu/DingTalk) for "definition of done" alerts only

4. Read my codebase structure and create agent spawn templates appropriate for my tech stack
```

OpenClaw analizará la base de código, creará scripts, configurará monitoreo Cron. Aproximadamente 10 minutos completa la configuración inicial.

## Consejos prácticos

- **Comenzar pequeño**: Primero ejecutar flujo completo con 1-2 Agentes (Spawn → Codificación → PR → Revisión → Merge), confirmar que todo funciona normal antes de expandir
- **RAM es el verdadero cuello de botella**: Cada Agente necesita `node_modules` y procesos de construcción independientes. 16GB de memoria máximo pueden ejecutar 4-5 Agentes simultáneamente, y no pueden disparar construcción al mismo tiempo. Se recomienda 64GB+ de memoria para paralelismo a gran escala
- **Referencia de costo**: Claude ~$100/mes + Codex ~$90/mes, se puede comenzar con $20/mes para入门
- **Definición de Terminado es clave**: Agentes deben saber "PR creado ≠ completado", debe incluir CI pasa, revisión pasa, capturas (si aplica)
- **tmux es mejor que modo `-p`**: tmux soporta enviar instrucciones a mitad de camino para corregir, no necesita matar y reiniciar Agente
- **No sondear Agentes directamente**: Usar scripts determinísticos para verificar estado externo de tmux / PR / CI, consumo extremadamente bajo de Tokens
- **Regla de capturas de UI**: Forzar que PR incluya capturas puede acortar significativamente tiempo de revisión — ver capturas es mucho más rápido que abrir entorno de preview

## Efecto real

Datos de entorno de producción del autor de 4 semanas (usado para producto B2B SaaS real):

- Máximo 94 commits en un día (hubo 3 reuniones de clientes ese día, no abrió editor)
- Promedio diario aprox. 50 commits
- 7 PRs completados en 30 minutos (desde idea hasta producción)
- Tasa de aprobación de una vez extremadamente alta para tareas pequeñas y medianas, casi no necesita intervención humana
- Entrega de solicitudes de funciones el mismo día → mejora conversión de leads a clientes pagantes

## Enlaces relacionados

- [Artículo original (X/Twitter)](https://x.com/elvissun/status/2025920521871716562) — Artículo completo de Elvis Sun
- [Versión en chino (Zhihu)](https://zhuanlan.zhihu.com/p/2010127051726792220) — Incluye gráficos completos
- [Repositorio oficial de OpenClaw](https://github.com/openclaw/openclaw)
- [Anthropic: Construir Agentes efectivos](https://www.anthropic.com/research/building-effective-agents)

## Adaptación para usuarios de China

### Reemplazo de canales de notificación

| Esquema original | Reemplazo local | Descripción |
|--------|----------|------|
| Telegram | **Bot personalizado de Feishu** | Push vía Webhook, se puede llamar con `curl`, referencia [Asistente IA de Feishu](cn-feishu-ai-assistant.md) |
| Telegram | **Bot personalizado de DingTalk** | Soporta Webhook + verificación de firma, referencia [Asistente IA de DingTalk](cn-dingtalk-ai-assistant.md) |
| Telegram | **Mensaje de aplicación de WeCom** | Push vía API de aplicación, referencia [Asistente IA de WeCom](cn-wecom-ai-assistant.md) |

### Reemplazo de monitoreo y rastreo de errores

| Esquema original | Reemplazo local | Descripción |
|--------|----------|------|
| Sentry | **Alibaba Cloud ARMS** | Monitoreo en tiempo real de aplicaciones, soporta activar Agentes con alertas automáticas |
| Sentry | **Monitoreo de rendimiento frontend de Tencent Cloud** | Reporte de errores y rastreo de rendimiento |

### Alojamiento de código y CI

- **GitHub + gh CLI** (recomendado mantener): CLI `gh` está disponible en China, puede necesitar proxy para acelerar
- **Si usas Gitee**: Comando `gh` en `check-agents.sh` necesita reemplazarse con llamadas a API de Gitee, cambiar CI a Jenkins, etc.

### Obsidian Vault

Obsidian se puede usar normalmente en China, no necesita reemplazo. Si el equipo usa Yuque o documentos de Feishu, se puede sincronizar actas de reuniones a sistema de archivos local vía API para que el orquestador lea.

### Disponibilidad de Codex

CLI de Codex necesita acceso a API de OpenAI, China necesita VPN. Si no se puede usar, se puede cambiar todo a Claude Code:

```text
# Cambiar todas las tareas a ejecución de Claude Code
claude --model claude-opus-4.5 \
  --dangerously-skip-permissions \
  -p "Your prompt here"
```

Tareas de Codex en guía de selección de modelos se pueden asignar a `claude-opus-4.5` (razonamiento profundo) o `claude-sonnet-4.5` (priorizar velocidad).

### Prompt de adaptación de notificación Feishu

El siguiente prompt reemplaza notificación de Telegram por Feishu:

```text
## Configuración de Notificación (Webhook Feishu)

Cuando un PR pasa TODOS los checks (CI verde + 3 revisiones de código aprobadas + capturas si hay cambios de UI):
1. POST a webhook de Feishu: $FEISHU_WEBHOOK_URL
2. Payload del mensaje:
   {
     "msg_type": "interactive",
     "card": {
       "header": {"title": {"content": "PR #{number} Listo para Revisión", "tag": "plain_text"}},
       "elements": [{"tag": "div", "text": {"content": "**{title}**\nRama: {branch}\nCI: ✅ Revisiones: ✅", "tag": "lark_md"}}]
     }
   }
3. SOLO notificar en "definición de terminado" — nunca en estados intermedios
```

---

**Fuente de inspiración**: [Elvis Sun](https://x.com/elvissun/status/2025920521871716562) — Fundador independiente, construye producto B2B SaaS con flota de Agentes Codex + Claude Code orquestados por OpenClaw (PR Agéntico), promedio diario de 50+ commits, completa carga de trabajo de equipo de desarrollo él solo. Lo describe como "de gestionar Claude Code, a gestionar un Agente de OpenClaw que gestiona flota de Claude Code y Codex".

**Enlace original**: [Elvis Sun en X](https://x.com/elvissun/status/2025920521871716562) · [Versión en chino de Zhihu](https://zhuanlan.zhihu.com/p/2010127051726792220)
