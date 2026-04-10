# Orquestación de Flujo de Trabajo n8n con OpenClaw

Dejar que tu agente de AI (agent) gestione directamente claves de API y llame a servicios externos es un caldo de cultivo para accidentes de seguridad. Cada nueva integración significa una credencial más en `.env.local`, y una superficie de ataque más que el agente podría filtrar accidentalmente o usar mal.

Este caso de uso describe un patrón: OpenClaw delega todas las interacciones con APIs externas a flujos de trabajo de n8n vía webhooks — el agente nunca toca las credenciales, cada integración se puede inspeccionar visualmente y bloquear.

## Dolor

Cuando OpenClaw maneja todo directamente, te enfrentas a tres problemas superpuestos:

- **Falta de visibilidad**: Cuando la lógica está enterrada en archivos de skills de JavaScript o scripts shell, es difícil verificar qué construyó realmente el agente
- **Propagación de credenciales**: Cada clave de API existe en el entorno del agente, un commit erróneo podría exponerla
- **Tokens desperdiciados**: Tareas deterministas (enviar correos, actualizar hojas de cálculo) podrían ejecutarse como flujos de trabajo simples, en lugar de consumir tokens de razonamiento de LLM

## Descripción general de funciones

- **Modo proxy**: OpenClaw escribe flujos de trabajo de n8n con webhooks entrantes, luego realiza todas las interacciones subsiguientes con APIs llamando a estos webhooks
- **Aislamiento de credenciales**: Las claves de API se almacenan en el almacén de credenciales de n8n — el agente solo conoce las URLs de webhook
- **Depuración visual**: Cada flujo de trabajo se puede inspeccionar en la UI de arrastrar y soltar de n8n
- **Flujos de trabajo bloqueables**: Después de construir y probar el flujo de trabajo, puedes bloquearlo para evitar que el agente modifique cómo interactúa con las APIs
- **Pasos de seguridad a prueba de fallos**: Puedes agregar validación, límites de tasa y compuertas de aprobación en n8n, interceptando antes de que se ejecute cualquier llamada externa

## Cómo funciona

1. **El agente diseña el flujo de trabajo**: Dile a OpenClaw qué necesitas (por ejemplo, "crea un flujo de trabajo que envíe un mensaje de Slack cuando un nuevo issue de GitHub se marque como `urgent`")
2. **El agente construye en n8n**: OpenClaw crea el flujo de trabajo vía la API de n8n, incluyendo un trigger de webhook entrante
3. **Tú agregas credenciales**: Abres la UI de n8n, manualmente agregas tu token de Slack / token de GitHub
4. **Tú bloqueas el flujo de trabajo**: Evita que el agente modifique más
5. **El agente llama al webhook**: De ahora en adelante, OpenClaw llama a `http://n8n:5678/webhook/my-workflow` con un payload JSON — nunca ve las claves de API

```text
┌──────────────┐     Llamada de webhook     ┌─────────────────┐     Llamada de API    ┌──────────────┐
│   OpenClaw   │ ───────────────────→  │   Flujo de trabajo   │ ─────────────→  │   Servicio    │
│  （Agente）   │   （Sin credenciales）   │   n8n             │  （Credenciales    │   Externo     │
│              │                       │ （Bloqueado, con   │   se quedan     │ （Slack, etc）│
│              │                       │   claves de API）   │   aquí）        │              │
└──────────────┘                       └─────────────────┘                  └──────────────┘
```

## Habilidades requeridas

- Acceso a API de `n8n` (para crear/trigger flujos de trabajo)
- `fetch` o `curl`, para llamadas de webhook
- Docker (si usas el stack preconfigurado)
- Gestión de credenciales de n8n (operación manual, solo necesita configurarse una vez por integración)

## Cómo configurar

### Esquema Uno: Stack de Docker preconfigurado

Una configuración de Docker Compose mantenida por la comunidad ([openclaw-n8n-stack](https://github.com/caprihan/openclaw-n8n-stack)) pre-conecta todos los componentes en una red Docker compartida:

```bash
git clone https://github.com/caprihan/openclaw-n8n-stack.git
cd openclaw-n8n-stack
cp .env.template .env
# Agrega tu clave de API de Anthropic a .env
docker-compose up -d
```

Esto te proporciona:
- OpenClaw ejecutándose en puerto 3456
- n8n ejecutándose en puerto 5678
- Red Docker compartida, permitiendo que OpenClaw llame directamente a `http://n8n:5678/webhook/...`
- Plantillas de flujos de trabajo pre-construidas (verificación de hechos multi-LLM, clasificación de correos, monitoreo social)

### Esquema Dos: Configuración manual

1. Instalar n8n (`npm install n8n -g` o ejecutar vía Docker)
2. Configurar OpenClaw para que sepa la URL base de n8n
3. Agregar el siguiente contenido a tu AGENTS.md:

```text
## Patrón de Integración n8n

When I need to interact with external APIs:

1. NEVER store API keys in my environment or skill files
2. Check if an n8n workflow already exists for this integration
3. If not, create one via n8n API with a webhook trigger
4. Notify the user to add credentials and lock the workflow
5. For all future calls, use the webhook URL with a JSON payload

Workflow naming: openclaw-{service}-{action}
Example: openclaw-slack-send-message

Webhook call format:
curl -X POST http://n8n:5678/webhook/{workflow-name} \
  -H "Content-Type: application/json" \
  -d '{"channel": "#general", "message": "Hello from OpenClaw"}'
```

## Ideas clave

- **Mata tres pájaros de un tiro**: Observabilidad (UI visual), Seguridad (aislamiento de credenciales) y Rendimiento (flujos de trabajo deterministas no consumen tokens)
- **Bloquear después de probar**: El ciclo "Construir → Probar → Bloquear" es crucial — sin bloquear, el agente puede modificar flujos de trabajo sigilosamente
- **n8n tiene 400+ integraciones**: La mayoría de los servicios externos que quieres conectar ya tienen nodos de n8n, ahorrándole al agente escribir llamadas de API personalizadas
- **Pista de auditoría gratuita**: n8n registra datos de entrada/salida de cada ejecución de flujo de trabajo

## Fuentes de inspiración

Este patrón fue descrito por [Simon Hoiberg](https://x.com/SimonHoiberg/status/2020843874382487959), quien articuló tres razones por las que este método es superior a dejar que OpenClaw maneje las interacciones con APIs directamente: Observabilidad vía la UI visual de n8n, Seguridad vía aislamiento de credenciales, y Rendimiento vía ejecución de subtareas deterministas como flujos de trabajo en lugar de llamadas de LLM. El repositorio [openclaw-n8n-stack](https://github.com/caprihan/openclaw-n8n-stack) proporciona una configuración de Docker Compose lista para usar para implementar este patrón.

## Enlaces relacionados

- [Documentación de n8n](https://docs.n8n.io/)
- [openclaw-n8n-stack (Configuración de Docker)](https://github.com/caprihan/openclaw-n8n-stack)
- [Documentación de trigger de webhook de n8n](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.webhook/)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/n8n-workflow-orchestration.md)
