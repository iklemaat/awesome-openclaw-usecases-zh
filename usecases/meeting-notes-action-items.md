# Automatización de Actas de Reuniones y Tareas Pendientes

> Incluye adaptación local: Feishu Minutes / Tencent Meeting / DingTalk

Acabas de terminar una reunión de equipo de 45 minutos, necesitas escribir actas, extraer elementos de acción, y luego ingresarlos manualmente a Jira, Linear o lista de tareas pendientes — todo manualmente. Cuando terminas de procesar, la siguiente reunión ya comenzó. ¿Y si tu agente pudiera encargarse automáticamente de todo esto en el momento en que la transcripción del texto aterriza?

Este caso de uso convierte cualquier transcripción de reunión en actas estructuradas, y crea automáticamente tareas correspondientes en herramientas de gestión de proyectos.

## Dolor

Las actas de reuniones son tediosas pero importantes. La mayoría de las personas o las saltan (y luego pierden contexto), o gastan más de 20 minutos escribiéndolas manualmente. Los elementos de acción frecuentemente se olvidan — porque solo existen en la cabeza de alguien, o están enterrados en alguna esquina del historial de chat. Este agente elimina la brecha entre "lo discutimos" y "registrado, asignado".

## Qué puede hacer

| Función | Descripción |
|------|------|
| **Escuchar fuentes de transcripción** | Soporta exportación de Otter.ai, transcripción de Google Meet, resumen de grabación de Zoom, o pegar directamente en el chat |
| **Extraer información clave** | Identificar decisiones, temas discutidos, elementos de acción con sus responsables y fechas límite |
| **Crear tareas automáticamente** | Crear tareas en Jira, Linear, Todoist o Notion, asignar a la persona correcta, adjuntar contexto de reunión |
| **Publicar resumen de actas** | Empujar resumen a Slack o Discord, dejar registro para todo el equipo |
| **Seguimiento y recordatorio** | Opcional:催促 responsable vía recordatorio programado antes de la fecha límite |

## Habilidades requeridas

- Integración con plataforma de gestión de proyectos: Jira, Linear, Todoist o Notion (para creación de tareas)
- Integración con herramienta de comunicación de equipo: Slack o Discord (para publicar resumen)
- Acceso a sistema de archivos (para leer archivos de transcripción)
- Tareas programadas / cron (para seguimiento y recordatorios)
- Opcional: Otter.ai, Fireflies.ai o API de Google Meet (para obtener automáticamente texto de transcripción)

## Cómo configurar

### 1. Elegir fuente de transcripción

La forma más simple es pegar directamente el texto de transcripción en el chat. Si necesitas automatización, se puede configurar monitoreo de carpeta o integración con API.

### 2. Uso básico: Pegar transcripción y generar actas

```text
Acabo de terminar una reunión. Aquí está la transcripción:

[pegar transcripción o apuntar a archivo]

Por favor:
1. Escribe un resumen conciso (máximo 5 puntos) cubriendo decisiones clave y temas.
2. Extrae TODOS los elementos de acción. Para cada uno, identifica:
   - Qué necesita hacerse
   - Quién es responsable (coincidir nombres con mi equipo)
   - Fecha límite (si se menciona, si no marcar como "TBD")
3. Crea un ticket de Jira para cada elemento de acción, asignado a la persona correcta.
4. Publica el resumen completo en #meeting-notes en Slack.
```

### 3. Avanzado: Monitoreo automático de carpeta

Configurar un pipeline automatizado que escanea regularmente archivos de transcripción y procesa:

```text
Configura una tarea recurrente: cada 30 minutos, verifica ~/meeting-transcripts/ 
para nuevos archivos .txt o .vtt. Cuando encuentres uno:

1. Analiza la transcripción en un resumen estructurado con elementos de acción.
2. Crea tareas en Linear para cada elemento de acción.
3. Publica el resumen en #team-updates en Slack.
4. Mueve el archivo procesado a ~/meeting-transcripts/processed/.

Para cada elemento de acción con fecha límite, configura un recordatorio para 
notificar al asignado en Slack un día antes de que venza.
```

### 4. Formato de salida personalizado

```text
Al escribir resúmenes de reuniones, usa siempre esta estructura:
- **Fecha y Asistentes** en la parte superior
- **Decisiones Clave** — lista numerada de lo que se decidió
- **Elementos de Acción** — tabla con columnas: Tarea, Responsable, Fecha Límite, Estado
- **Preguntas Abiertas** — cualquier cosa no resuelta que necesita seguimiento
```

## Ideas clave

- El verdadero valor no está en las actas en sí, sino en **crear tareas automáticamente**. Actas de reuniones que no se convierten en tareas rastreables son solo formalismo
- Se puede combinar con el caso de uso [Gestor de Tareas Todoist](todoist-task-manager.md), obtener visualización completa de tareas creadas por el agente
- Archivos de subtítulos VTT/SRT de Zoom o Google Meet son buena entrada — tienen marcas de tiempo, ayudan al agente a atribuir declaraciones a personas específicas
- Comenzar con lo más simple primero (pegar transcripción, obtener resumen), verificar calidad de salida antes de automatizar gradualmente. No sobre-diseñar pipelines antes de verificar efectos

## Adaptación para usuarios de China

Los escenarios de reuniones y ecosistema de herramientas de empresas nacionales son muy diferentes de los extranjeros. Los siguientes son esquemas de adaptación para el entorno nacional.

### Puntos dolorosos de reuniones nacionales

Según estadísticas, las pérdidas causadas por baja eficiencia de reuniones en empresas nacionales promedian 11% de ingresos anuales,其中 el manejo inadecuado de actas de reuniones es una de las principales causas. Los puntos dolorosos comunes incluyen:

- **Demasiadas reuniones**: Los empleados se ven obligados a asistir a muchas reuniones innecesarias, carga pesada de actas
- **Desconexión entre actas y ejecución**: Actas escritas nadie las ve, elementos de acción dispersos en grupos de chat nadie da seguimiento
- **Herramientas de transcripción fragmentadas**: Feishu Minutes, resúmenes inteligentes de Tencent Meeting, DingTalk AI Listening cada uno tiene su propio ecosistema cerrado, datos de actas difíciles de fluir cross-plataforma

### Esquemas alternativos de fuentes de transcripción

| Herramienta original | Reemplazo nacional | Descripción |
|---------|---------|------|
| Otter.ai / Fireflies.ai | **Feishu Minutes (妙记)** | Soporta 19 idiomas, precisión 98%, puede exportar formato documento Feishu/TXT/SRT |
| Transcripción de Google Meet | **Resumen inteligente de Tencent Meeting** | Proporciona API REST para obtener párrafos de transcripción y resumen de actas (necesita versión comercial/empresarial) |
| Resumen de grabación de Zoom | **DingTalk AI Listening (钉钉 AI 听记)** | Soporta transcripción de 72 idiomas, puede sincronizar tareas pendientes a DingTalk Tasks con un clic |
| Pegar directamente | **Cualquier herramienta exporta y pega** | Forma más universal, no depende de plataforma específica |

### Esquemas alternativos de creación de tareas

| Herramienta original | Reemplazo nacional | Skill de OpenClaw |
|---------|---------|---------------|
| Jira / Linear | **Feishu Project (Meego)** | Escribir a documento Feishu vía [feishu-doc](https://playbooks.com/skills/openclaw/openclaw/feishu-doc), o usar API de Feishu Project |
| Todoist | **TickTick** | [ticktick-api-skill](https://playbooks.com/skills/openclaw/skills/ticktick-api-skill) |
| Notion | **Documento Feishu / Documento DingTalk** | [feishu-doc](https://playbooks.com/skills/openclaw/openclaw/feishu-doc) |
| Slack / Discord | **Grupo Feishu / Grupo DingTalk** | Enviar directamente vía plugins de canal Feishu/DingTalk |

### Esquema para usuarios de Feishu (recomendado)

El ecosistema de Feishu puede lograr el ciclo más completo: grabación de reunión → transcripción de Minutes → procesamiento de OpenClaw → documento Feishu + tareas.

**Pre-requisitos**: Completado el acceso básico de [Asistente IA de Feishu](cn-feishu-ai-assistant.md).

**Instalar skills relacionados con Feishu**:

```bash
npx playbooks add skill openclaw/openclaw --skill feishu-doc
npx playbooks add skill openclaw/skills --skill feishu-calendar-tool
```

**Modo de uso**: Después de terminar la reunión, exportar texto de transcripción desde Feishu Minutes (formato TXT o documento Feishu), luego enviar al robot de OpenClaw en Feishu:

```text
Esta es la transcripción de la reunión de revisión de productos de esta tarde:
[pegar texto exportado de Minutes]

Por favor:
1. Organizar en actas de reunión estructuradas (decisiones + elementos de acción + asuntos pendientes)
2. Escribir actas en documento Feishu, colocar en carpeta "Actas de Reunión"
3. Cada elemento de acción marcar responsable y fecha límite
4. Enviar resumen de actas al grupo de chat del equipo de producto
```

### Esquema para usuarios de Tencent Meeting

La plataforma abierta de Tencent Meeting proporciona API de transcripción de grabación (versión comercial/empresarial), se puede obtener automáticamente actas de reunión vía API:

- Consultar información de párrafos de transcripción de grabación: `GET /v1/records/{record_file_id}/transcripts`
- Consultar resumen inteligente: `GET /v1/smart/minutes/{record_file_id}`

Después de obtener texto de transcripción, se puede procesar más estructuralmente con OpenClaw y distribuir a herramientas de gestión de tareas correspondientes.

### Esquema para usuarios de DingTalk

DingTalk AI Listening soporta transcripción automática de grabaciones de reuniones, y puede sincronizar tareas pendientes a DingTalk Tasks con un clic. Después de la actualización de finales de 2025 soporta actas ilustradas y 72 idiomas.

**Pre-requisitos**: Completado el acceso básico de [Asistente IA de DingTalk](cn-dingtalk-ai-assistant.md).

**Modo de uso**: Después de terminar la reunión, exportar texto de actas desde DingTalk AI Listening, enviar al robot de DingTalk de OpenClaw para procesamiento adicional y distribución de tareas.

### Consejos prácticos

- **Verificar con la forma más simple primero**: No importa qué herramienta de reunión uses, primero copia manualmente el texto de transcripción y pégalo a OpenClaw, confirmar que la calidad de salida es satisfactoria antes de considerar automatización
- **Feishu Minutes + OpenClaw es actualmente la combinación más fluida**: Formato de exportación de Feishu Minutes es estandarizado, ecosistema de skills de Feishu de OpenClaw es el más maduro
- **Escenarios cross-plataforma**: Si el equipo usa múltiples herramientas de reuniones simultáneamente, unificar exportar a TXT y entregar a OpenClaw para procesar, usar misma plantilla para salida
- **Recordatorio de seguridad**: Las transcripciones de reuniones pueden contener información comercial sensible, asegura que OpenClaw se ejecute en entorno seguro, no enviar texto de transcripción a servicios de terceros no confiables

> **Consejo de seguridad**: Credenciales de API de Tencent Meeting y secretos de aplicación de Feishu son información sensible, por favor configura vía variables de entorno o archivo `.env`, asegura que `.env` se agregue a `.gitignore`.

## Enlaces relacionados

- [Otter.ai](https://otter.ai/)
- [API REST de Jira](https://developer.atlassian.com/cloud/jira/platform/rest/v3/)
- [API de Linear](https://developers.linear.app/)
- [API de Slack](https://api.slack.com/)
- [Feishu Minutes](https://www.feishu.cn/product/minutes) — Herramienta de transcripción de reuniones oficial de Feishu
- [Plataforma abierta de Tencent Meeting](https://meeting.tencent.com/open-api.html) — Documentación de API de Tencent Meeting
- [DingTalk AI Listening](https://www.dingtalk.com/markets/dingtalk-ai) — Función de transcripción de reuniones de DingTalk
- [Skill feishu-doc - ClawHub](https://playbooks.com/skills/openclaw/openclaw/feishu-doc)
- [Skill ticktick-api-skill - ClawHub](https://playbooks.com/skills/openclaw/skills/ticktick-api-skill)
- [Skill feishu-calendar-tool - ClawHub](https://playbooks.com/skills/openclaw/skills/feishu-calendar-tool)
