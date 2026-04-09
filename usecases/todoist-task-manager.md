# Gestor de Tareas Todoist: Visibilidad de Tareas del Agente

Maximiza la transparencia de flujos de trabajo de agentes de ejecución prolongada sincronizando razonamiento interno y registros de progreso directamente a Todoist.

## Dolor

Cuando los agentes ejecutan tareas complejas de múltiples pasos (como construir aplicaciones full-stack o realizar investigaciones profundas), los usuarios frecuentemente no pueden rastrear qué está haciendo el agente actualmente, qué pasos se completaron, y dónde puede estar atascado el agente. Para tareas en segundo plano, revisar manualmente registros de chat es tedioso.

## Qué puede hacer

Este caso de uso usa el skill `todoist-task-manager` para:
1. **Visualizar estado**: Crear tareas en secciones específicas, como `🟡 En Progreso` o `🟠 Esperando`.
2. **Externalizar razonamiento**: Publicar "planes" internos del agente en descripciones de tareas.
3. **Registros en streaming**: Agregar completado de sub-pasos como comentarios en tiempo real a tareas.
4. **Verificación automática**: Scripts de latido (heartbeat script) verifican tareas estancadas y notifican al usuario.

## Habilidades requeridas

No necesitas skills pre-construidos. Solo indica a tu agente de OpenClaw crear los scripts de bash descritos en **Guía de Configuración** abajo. Dado que OpenClaw puede gestionar su propio sistema de archivos y ejecutar comandos shell, "construirá" este skill según tus instrucciones.

## Guía de Configuración Detallada

### 1. Configurar Todoist

Crear un proyecto (por ejemplo "OpenClaw Workspace") y obtener su ID. Crear secciones para diferentes estados:
- `🟡 En Progreso`
- `🟠 Esperando`
- `🟢 Completado`

### 2. Implementar: Skill "auto-construido por el agente"

Puedes hacer que OpenClaw cree estos scripts para ti, en lugar de instalar skills. Cada script maneja diferentes partes de comunicación con la API de Todoist.

**`scripts/todoist_api.sh`** (script de encapsulamiento central):
```bash
#!/bin/bash
# Uso: ./todoist_api.sh <endpoint> <method> [data_json]
ENDPOINT=$1
METHOD=$2
DATA=$3
TOKEN="YOUR_TODOIST_API_TOKEN"

if [ -z "$DATA" ]; then
  curl -s -X "$METHOD" "https://api.todoist.com/rest/v2/$ENDPOINT" \
    -H "Authorization: Bearer $TOKEN"
else
  curl -s -X "$METHOD" "https://api.todoist.com/rest/v2/$ENDPOINT" \
    -H "Authorization: Bearer $TOKEN" \
    -H "Content-Type: application/json" \
    -d "$DATA"
fi
```

**`scripts/sync_task.sh`** (gestión de tareas y estado):
```bash
#!/bin/bash
# Uso: ./sync_task.sh <task_content> <status> [task_id] [description] [labels_json_array]
CONTENT=$1
STATUS=$2
TASK_ID=$3
DESCRIPTION=$4
LABELS=$5
PROJECT_ID="YOUR_PROJECT_ID"

case $STATUS in
  "In Progress") SECTION_ID="SECTION_ID_PROGRESS" ;;
  "Waiting")     SECTION_ID="SECTION_ID_WAITING" ;;
  "Done")        SECTION_ID="SECTION_ID_DONE" ;;
  *)             SECTION_ID="" ;;
esac

PAYLOAD="{\"content\": \"$CONTENT\""
[ -n "$SECTION_ID" ] && PAYLOAD="$PAYLOAD, \"section_id\": \"$SECTION_ID\""
[ -n "$PROJECT_ID" ] && [ -z "$TASK_ID" ] && PAYLOAD="$PAYLOAD, \"project_id\": \"$PROJECT_ID\""
if [ -n "$DESCRIPTION" ]; then
  # Escapar saltos de línea y comillas en descripción
  ESC_DESC=$(echo "$DESCRIPTION" | sed ':a;N;$!ba;s/\n/\\n/g' | sed 's/"/\\"/g')
  PAYLOAD="$PAYLOAD, \"description\": \"$ESC_DESC\""
fi
[ -n "$LABELS" ] && PAYLOAD="$PAYLOAD, \"labels\": $LABELS"
PAYLOAD="$PAYLOAD}"

if [ -n "$TASK_ID" ]; then
  ./scripts/todoist_api.sh "tasks/$TASK_ID" POST "$PAYLOAD"
else
  ./scripts/todoist_api.sh "tasks" POST "$PAYLOAD"
fi
```

**`scripts/add_comment.sh`** (registro de progreso):
```bash
#!/bin/bash
# Uso: ./add_comment.sh <task_id> <comment_text>
TASK_ID=$1
TEXT=$2
# Escapar saltos de línea y comillas en texto
ESC_TEXT=$(echo "$TEXT" | sed ':a;N;$!ba;s/\n/\\n/g' | sed 's/"/\\"/g')
PAYLOAD="{\"task_id\": \"$TASK_ID\", \"content\": \"$ESC_TEXT\"}"
./scripts/todoist_api.sh "comments" POST "$PAYLOAD"
```

### 3. Prompt de uso

Puedes entregar el siguiente prompt a tu agente, para **configurar** y **usar** el sistema de visibilidad de tareas:

```text
I want you to build a Todoist-based task visibility system for your own runs.

First, create three bash scripts in a 'scripts/' folder:
1. todoist_api.sh (a curl wrapper for Todoist REST API)
2. sync_task.sh (to create or update tasks with specific section_ids for In Progress, Waiting, and Done)
3. add_comment.sh (to post progress logs as comments)

Use these variables for the setup:
- Token: [Your Todoist API Token]
- Project ID: [Your Project ID]
- Section IDs: [In Progress ID, Waiting ID, Done ID]

Once created, for every complex task I give you:
1. Create a task in 'In Progress' with your full PLAN in the description.
2. For every sub-step completion, call add_comment.sh with a log of what you did.
3. Move the task to 'Done' when finished.
```

## Enlaces relacionados

- [Documentación de API REST de Todoist](https://developer.todoist.com/rest/v2/)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/todoist-task-manager.md)
