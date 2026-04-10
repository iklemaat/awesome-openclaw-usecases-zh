# Servidor Casero Auto-Reparable y Gestión de Infraestructura

Ejecutar un servidor casero significa estar disponible 24/7 para manejar problemas de tu propia infraestructura. Los servicios se caen a las 3 AM, los certificados expiran silenciosamente, el espacio en disco se agota, los Pods entran en ciclos de fallo (crash-loop) — y todo esto sucede mientras duermes o estás fuera.

Este caso de uso convierte OpenClaw en un agente de infraestructura de ejecución persistente, con acceso SSH, tareas programadas automatizadas (cron jobs), y la capacidad de detectar, diagnosticar y reparar problemas antes de que tú tengas que lidiar con ellos.

## Dolor

Los entusiastas de laboratorios caseros y usuarios auto-alojados enfrentan una carga de mantenimiento continua:

- Las verificaciones de salud, monitoreo de registros y alertas requieren configuración manual y atención continua
- Cuando ocurren fallos, debes iniciar sesión vía SSH, diagnosticar y reparar — a menudo desde tu teléfono
- Las herramientas de infraestructura como código (Terraform, Ansible, listas de Kubernetes) necesitan actualizaciones periódicas
- El conocimiento sobre tu configuración vive en tu cabeza, no en documentación buscable
- Las tareas diarias (clasificación de correos, verificaciones de despliegue, auditorías de seguridad) consumen horas cada semana

## Descripción general de funciones

- **Monitoreo automatizado de salud**: Verifica servicios, despliegues y recursos del sistema basado en tareas programadas
- **Auto-reparación**: Detecta problemas mediante verificaciones de salud, aplica reparaciones autónomamente (reiniciar Pods, escalar recursos, reparar configuraciones)
- **Gestión de infraestructura**: Escribe y aplica listas de Terraform, Ansible y Kubernetes
- **Resumen matutino**: Resumen diario de salud del sistema, calendario, clima y estado del tablero de tareas
- **Clasificación de correos**: Escanea la bandeja de entrada, marca elementos accionables, archiva ruido
- **Extracción de conocimiento**: Exporta notas y conversaciones para procesar como base de conocimientos estructurada y buscable
- **Pipeline de publicación de blog**: Borrador → generar imagen de portada → publicar en CMS → desplegar a plataforma de hosting — completamente automatizado
- **Auditoría de seguridad**: Escanea regularmente claves hardcodeadas, contenedores privilegiados y permisos excesivamente permisivos

## Habilidades requeridas

- Acceso `ssh` a máquinas en red doméstica
- `kubectl`, para gestión de clúster de Kubernetes
- `terraform` y `ansible`, para infraestructura como código
- CLI de `1password`, para gestión de claves
- CLI `gog`, para acceso a correos
- Acceso a API de calendario
- Vault de Obsidian o directorio de notas (para base de conocimientos)
- `openclaw doctor`, para autodiagnóstico

## Cómo configurar

### 1. Configuración de agente central

Nombra tu agente y define su alcance de acceso en AGENTS.md:

```text
## Infrastructure Agent

You are Reef, an infrastructure management agent.

Access:
- SSH to all machines on the home network (192.168.1.0/24)
- kubectl for the K3s cluster
- 1Password vault (read-only for credentials, dedicated AI vault)
- Gmail via gog CLI
- Calendar (yours + partner's)
- Obsidian vault at ~/Documents/Obsidian/

Rules:
- NEVER hardcode secrets — always use 1Password CLI or environment variables
- NEVER push directly to main — always create a PR
- Run `openclaw doctor` as part of self-health checks
- Log all infrastructure changes to ~/logs/infra-changes.md
```

### 2. Sistema de tareas programadas automatizadas

El núcleo de esta configuración es el sistema de tareas programadas. Configurar en HEARTBEAT.md:

```text
## Cron Schedule

Every 15 minutes:
- Check kanban board for in-progress tasks → continue work

Every hour:
- Monitor health checks (Gatus, ArgoCD, service endpoints)
- Triage Gmail (label actionable items, archive noise)
- Check for unanswered alerts or notifications

Every 6 hours:
- Knowledge base data entry (process new Obsidian notes)
- Self health check (openclaw doctor, disk usage, memory, logs)

Every 12 hours:
- Code quality and documentation audit
- Log analysis via Loki/monitoring stack

Daily:
- 4:00 AM: Nightly brainstorm (explore connections between notes)
- 8:00 AM: Morning briefing (weather, calendars, system stats, task board)
- 1:00 AM: Velocity assessment (process improvements)

Weekly:
- Knowledge base QA review
- Infrastructure security audit
```

### 3. Configuración de seguridad (crítico)

Esto no es negociable. Antes de dar acceso SSH a tu agente:

```text
## Security Checklist

1. Pre-push hooks:
   - Install TruffleHog or similar secret scanner on ALL repositories
   - Block any commit containing hardcoded API keys, tokens, or passwords

2. Local-first Git workflow:
   - Use Gitea (self-hosted) for private code before pushing to public GitHub
   - CI scanning pipeline (Woodpecker or similar) runs before any public push
   - Human review required before main branch merges

3. Defense in depth:
   - Dedicated 1Password vault for AI agent (limited scope)
   - Network segmentation for sensitive services
   - Daily automated security audits checking for:
     * Privileged containers
     * Hardcoded secrets in code or configs
     * Overly permissive file/network access
     * Known vulnerabilities in deployed images

4. Agent constraints:
   - Branch protection: PR required for main, agent cannot override
   - Read-only access where write isn't needed
   - All changes logged and auditable via git
```

### 4. Plantilla de resumen matutino

```text
## Daily Briefing Format

Generate and deliver at 8:00 AM:

### Weather
- Current conditions and forecast for [your location]

### Calendars
- Your events today
- Partner's events today
- Conflicts or overlaps flagged

### System Health
- CPU / RAM / Storage across all machines
- Services: UP/DOWN status
- Recent deployments (ArgoCD)
- Any alerts in last 24h

### Task Board
- Cards completed yesterday
- Cards in progress
- Blocked items needing attention

### Highlights
- Notable items from nightly brainstorm
- Emails requiring action
- Upcoming deadlines this week
```

## Ideas clave

- **"No pensé que tendría un servidor auto-reparable"**: Los agentes pueden ejecutar comandos SSH, Terraform, Ansible y kubectl, reparando fallos de infraestructura antes de que siquiera sepas que hay un problema
- **La IA hardcodeará claves**: Este es el riesgo de seguridad número uno. Si no configuras salvaguardas, la IA escribirá claves de API en línea en el código sin dudarlo. Los hooks de pre-push y el escaneo de claves son esenciales
- **El flujo de trabajo Git local-first es crucial**: Nunca dejes que el agente empuje directamente a repositorios públicos. Usa una instancia privada de Gitea como área de preparación,配合 CI scanning
- **Las tareas programadas son el producto real**: La automatización programada (verificaciones de salud, clasificación de correos, resúmenes) proporciona más valor diario que comandos ocasionales
- **La extracción de conocimiento tiene efecto compuesto**: Procesar notas, exportaciones de conversaciones y correos en una base de conocimientos estructurada se vuelve cada vez más valiosa con el tiempo — un usuario extrajo 49,079 hechos atómicos solo del historial de ChatGPT

## Fuentes de inspiración

Este caso de uso se basa en el artículo detallado de Nathan ["Everything I've Done with OpenClaw (So Far)"](https://madebynathan.com/2026/02/03/everything-ive-done-with-openclaw-so-far/), donde describe su agente de OpenClaw "Reef" ejecutándose en un servidor casero, con acceso SSH a todas las máquinas, un clúster de Kubernetes, integración con 1Password, y un vault de Obsidian con 5,000+ notas. Reef ejecuta 15 tareas programadas activas, 24 scripts personalizados, y construye y despliega autónomamente múltiples aplicaciones, incluyendo una UI de gestión de tareas. Nathan aprendió una dura lección el primer día después de una fuga de clave de API: "Los asistentes de AI no dudan en hardcodear claves. A veces no tienen el mismo instinto de seguridad que los humanos." Su configuración de seguridad de defensa en profundidad (hooks de pre-push de TruffleHog, Gitea local, escaneo CI, auditorías diarias) es una lectura esencial para cualquiera que intente este patrón.

También verificado en [OpenClaw Showcase](https://openclaw.ai/showcase), `@georgedagg_` describió un patrón similar: monitoreo de despliegue, revisión de registros, reparación de configuraciones y envío de PRs — todo mientras paseaba al perro.

## Enlaces relacionados

- [Artículo completo de Nathan](https://madebynathan.com/2026/02/03/everything-ive-done-with-openclaw-so-far/)
- [Documentación de OpenClaw](https://github.com/openclaw/openclaw)
- [TruffleHog (Escaneo de claves)](https://github.com/trufflesecurity/trufflehog)
- [K3s (Kubernetes ligero)](https://k3s.io/)
- [Gitea (Git auto-alojado)](https://gitea.io/)
- [n8n (Automatización de flujos de trabajo)](https://n8n.io/)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/self-healing-home-server.md)
