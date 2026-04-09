# Arquitectura Multi-Agente de E-commerce: Desde Consulta de Datos hasta Operación Automática de Cadena Completa

> **Niveles técnicos divididos**: Versión单机 (Feishu + Skills) solo necesita experiencia básica de línea de comandos ⭐⭐; Despliegue SaaS multi-tenant necesita experiencia en Kubernetes y cloud native ⭐⭐⭐.

La operación diaria de vendedores de e-commerce se dispersa en múltiples escenarios como consultas de ventas, monitoreo de inventario, manejo de servicio al cliente. Las herramientas de asistencia integradas de plataformas tradicionales están predefinidas por la plataforma, los canales están bloqueados en el backend, solo pueden esperar pasivamente a que preguntes. El equipo de AWS China completó un PoC (prueba de concepto) completo con OpenClaw — cubrir escenarios principales con 4 Skills de nivel de plataforma, multi-Agente vinculados a diferentes grupos de Feishu logran división de roles, Cron envía activamente perspectivas operativas, convirtiendo la construcción de capacidades de "compilación" a "interpretación" — escrito y usado, modificado y生效.

## Qué puede hacer

- **Entrada de datos unificada**: Una ventana de conversación cubre todos los datos de ventas, inventario, clientes, productos — "¿Cuánto se vendió hoy?" responde directamente, no necesita cambiar a backend
- **Skill es capacidad**: Usar archivos Markdown define lógica de consulta de datos, personal de operaciones que sabe escribir `curl + jq` puede encapsular, lanzar en minutos
- **División de roles multi-Agente**: Asistente de ventas vinculado a grupo de operaciones, servicio postventa vinculado a grupo de servicio al cliente, cada uno solo carga Skills y personalidad necesarios, no interfieren
- **Notificación activa**: Inventario casi agotado, tasa de devoluciones sube anormalmente — la IA te lo dice activamente a través de Cron, no necesitas preguntar
- **Incrustación multicanal**: Feishu, Slack, WebChat, Telegram comparten el mismo motor de IA y Skills
- **Costo controlable**: Consultas simples aprox. $0.08/vez, análisis complejos aprox. $0.12/vez (PoC real)

## Habilidades requeridas

| Skill | Fuente | Uso |
|------|------|------|
| sales-query | Skill auto-construido | Panorama de ventas, lista de pedidos, análisis de devoluciones, gráficos de tendencias |
| product-ranking | Skill auto-construido | Lista de más vendidos, productos estancados, rendimiento de nuevos productos, distribución por categoría |
| inventory-alert | Skill auto-construido | Alerta de inventario bajo, recordatorio de agotamiento, sugerencias de reabastecimiento |
| customer-insight | Skill auto-construido | Panorama de clientes, análisis de nuevos/clientes recurrentes, distribución geográfica |
| Canal Feishu | Canal integrado | Vinculación de múltiples grupos, modo WebSocket (conexión larga) |
| Tarea programada Cron | Función integrada | Alerta de inventario, informe diario matutino, monitoreo de anomalías |

> Los 4 Skills necesitan desarrollarse según la API de tu plataforma de e-commerce. El PoC de AWS proporciona una implementación de referencia completa basada en Mock API, incluye 12 SKUs, 50 clientes, 20+ endpoints.

### Estado actual de acceso a API

**Plataformas de e-commerce nacionales de China (JD/Taobao/Pinduoduo) actualmente no tienen SDKs de API open source de 1000+ stars.** Los SDKs oficiales se distribuyen a través de portales de desarrolladores de cada plataforma, no se alojan en GitHub, y todas las APIs de plataformas requieren autenticación de comerciante, verificación de nombre real y revisión de aplicaciones:

| Plataforma | Portal de Desarrollador | Requisitos de Acceso a API |
|------|-----------|-------------|
| JD | [jos.jd.com](https://jos.jd.com/) | Calificación empresarial + vinculación de tienda, aplicación auto-desarrollada solo para tiendas de esta empresa |
| Taobao/Tmall | [open.taobao.com](https://open.taobao.com/) | Calificación empresarial, APIs de transacción necesitan revisión adicional, algunas categorías cerradas |
| Pinduoduo | [open.pinduoduo.com](https://open.pinduoduo.com/) | 6 roles, mayoría necesita calificación empresarial, revisión 1-3 días hábiles |
| **Shopify** | [shopify.dev](https://shopify.dev/) | Cuenta Partner (gratis), API completamente abierta |

**Recomendado para e-commerce transfronterizo: Shopify**: SDK oficial de Python ([Shopify/shopify_python_api](https://github.com/Shopify/shopify_python_api), 1,400+ stars, mantenimiento activo) se puede usar directamente para construir Skills de OpenClaw, es actualmente la única plataforma de e-commerce con ecosistema open source maduro.

**Ruta real para plataformas nacionales**: Descargar SDKs oficiales desde portales de desarrolladores → completar autenticación de comerciante y autorización OAuth → encapsular como Skills de OpenClaw basado en SDKs. Mock API del PoC de AWS puede usarse como alternativa en fase de desarrollo, primero ejecutar la arquitectura, luego conectar APIs reales.

## Cómo configurar

### 1. Configuración de vinculación multi-Agente

Definir múltiples Agentes en `openclaw.json`, cada uno mapeado al grupo de Feishu correspondiente a través de Binding (enrutamiento):

```json
{
  "agents": {
    "list": [
      { "id": "sales", "name": "Asistente de Ventas", "workspace": "/data/ws/sales" },
      { "id": "support", "name": "Servicio Postventa", "workspace": "/data/ws/support" }
    ]
  },
  "bindings": [
    {
      "match": { "channel": "feishu", "peer": { "kind": "group", "id": "oc_abc111" } },
      "agentId": "sales"
    },
    {
      "match": { "channel": "feishu", "peer": { "kind": "group", "id": "oc_abc222" } },
      "agentId": "support"
    }
  ],
  "session": { "dmScope": "per-peer" }
}
```

> `dmScope: per-peer` significa que cada usuario (por `senderOpenId`) tiene un Session (sesión) independiente, historiales de conversación aislados entre sí.

**Resumen de arquitectura:**

| Grupo/Canal | Agente Vinculado | Skills Cargadas | Personalidad (SOUL.md) |
|---------|-----------|--------------|----------------|
| Feishu - Grupo de Operaciones de Ventas | sales | sales-query, product-ranking | Datos primero, conclusiones al inicio |
| Feishu - Grupo de Servicio Postventa | support | Manejo de reembolsos, consulta de tickets | Paciente y profesional, consuelo primero |
| Feishu - Chat privado | main (por defecto) | Todos los Skills | Asistente general |

Cada Agente posee directorio de Workspace independiente:

```
workspace-{agentId}/
├── AGENTS.md      ← Reglas de comportamiento
├── SOUL.md        ← Definición de personalidad
├── IDENTITY.md    ← Información de identidad
├── TOOLS.md       ← Permisos de herramientas
├── MEMORY.md      ← Memoria persistente
└── skills/
    ├── sales-query.SKILL.md
    └── order-lookup.SKILL.md
```

### 2. Definición de personalidad SOUL.md

**Plantilla de personalidad de Servicio Postventa:**

```markdown
# Servicio Postventa

Eres asistente de manejo postventa del equipo de e-commerce.

## Estilo
- Paciente, no impaciente
- Confirmar problema primero, luego dar solución
- Dar números claros y pasos de operación cuando involucre montos de reembolso

## Límites
- Solo maneja relacionado con postventa: reembolsos, cambios, tickets, quejas de clientes
- Para reembolsos que exceden monto autorizado, recordar al usuario buscar aprobación de supervisor
```

**Plantilla de personalidad de Asistente de Ventas:**

```markdown
# Asistente de Ventas

Eres asistente de análisis de datos del equipo de e-commerce.

## Estilo
- Datos primero, conclusiones al inicio
- Activamente da comparación interanual/mensual
- Activamente marca y da posibles causas cuando datos son anómalos

## Límites
- Solo maneja consultas relacionadas con ventas y productos
- No ejecuta operaciones de escritura como reembolsos, cambio de precios
```

### 3. Ejemplo de Skill de nivel de plataforma

Archivos Skill son documentos Markdown, describen lógica de consulta de datos. El siguiente es un ejemplo del Skill `sales-query`:

El siguiente prompt define el comportamiento y formato de salida del Skill de consulta de ventas (dado que está orientado a usuarios chinos, el contenido del Skill se escribe en chino):

```markdown
# sales-query

Consulta datos de ventas. Soporta filtrado por dimensiones como rango de fechas, categoría de producto, región, etc.

## Herramientas

Usar herramienta exec para ejecutar los siguientes comandos para obtener datos:

### Panorama de ventas de hoy

curl -s -H "Authorization: Bearer $API_TOKEN" \
  "$API_ENDPOINT/v1/orders/stats?date=$(date +%Y-%m-%d)" | jq '.'

### Lista de pedidos en rango de fechas específico

curl -s -H "Authorization: Bearer $API_TOKEN" \
  "$API_ENDPOINT/v1/orders?start_date={start}&end_date={end}&page=1" | jq '.orders'

## Formato de salida
- Montos usar símbolo ¥, dos decimales
- Cambios porcentuales marcar ↑ o ↓
- Usar tabla para mostrar cuando más de 10 datos
```

> **Idea clave**: Los límites de capacidad del Skill los decide la API — mientras haya API, se puede encapsular como Skill. De "ingenieros escriben código, definen Function Schema, prueban y lanzan" a "personal de operaciones escribe Markdown, pone en directorio skills,生效 inmediatamente".

### 4. Alerta de inventario programada con Cron

La siguiente configuración logra verificar inventario automáticamente cada 30 minutos y enviar alertas:

```json
{
  "kind": "every",
  "interval": "30m",
  "prompt": "Verificar estado de inventario, si hay productos agotados o de inventario bajo, listar detalles y sugerencias de reabastecimiento",
  "session": "isolated",
  "delivery": { "channel": "last" }
}
```

Más escenarios programados:

| Escenario | Configuración de programación | Uso |
|------|---------|------|
| Informe diario matutino | `cron: "0 8 * * *"` | Panorama de ventas de ayer + pedidos anómalos + recordatorio de reabastecimiento |
| Monitoreo en tiempo real | `every: "15m"` | Alerta de reembolsos de gran monto |
| Informe semanal | `cron: "0 9 * * 1"` | TOP 10 más vendidos + análisis de productos estancados |

### 5. Configuración de límites de seguridad

El control de seguridad en escenarios de e-commerce es especialmente importante — la IA puede consultar datos, pero no puede reembolsar o cambiar precios arbitrariamente.

**Lista blanca de ejecución de herramientas (safeBins):**

```json
{
  "tools": {
    "exec": {
      "host": "node",
      "security": "allowlist",
      "ask": "on-miss",
      "safeBins": ["jq", "grep", "curl", "cut", "sort", "uniq", "head", "tail", "tr", "wc"]
    }
  }
}
```

**Interruptores a nivel de Skill:**

```json
{
  "skills": {
    "entries": {
      "query-orders": { "enabled": true },
      "query-inventory": { "enabled": true },
      "refund": { "enabled": false },
      "change-price": { "enabled": false }
    }
  }
}
```

> **Importante**: `safeBins` opera a nivel de ejecutable, no puede distinguir `curl GET` (consulta) y `curl POST` (reembolso). Por lo tanto, **la aprobación de operaciones de escritura es responsabilidad de la API de negocio, no responsabilidad de la capa de IA** — se debe introducir un paso de confirmación manual en el lado de la API.

**Gestión de credenciales — Inyección de variables de entorno:**

```bash
# Credenciales de API se inyectan a través de variables de entorno, nunca hardcodeadas en scripts o archivos de configuración
export API_TOKEN="tu-api-token"
export API_ENDPOINT="https://api.your-platform.com"
```

### 6. Configuración de canal Feishu

OpenClaw usa modo WebSocket para conectar a Feishu, no necesita puertos de entrada públicos:

```json
{
  "channels": {
    "feishu": {
      "enabled": true,
      "appId": "cli_xxxxx",
      "appSecret": "xxxxx",
      "dmPolicy": "open",
      "groupPolicy": "open",
      "requireMention": true
    }
  }
}
```

**Pasos de configuración:**

1. Crear aplicación auto-construida empresarial en [Plataforma Abierta de Feishu](https://open.feishu.cn/), obtener App ID y App Secret
2. Configurar permisos: obtener mensajes de chat privado/grupal, obtener información de grupos, enviar mensajes, etc.
3. Habilitar capacidad de Bot
4. Escribir credenciales en `openclaw.json`
5. Iniciar instancia de OpenClaw — se conectará activamente a Feishu vía WebSocket
6. Volver a configurar suscripción de eventos en plataforma abierta de Feishu y lanzar aplicación

> **Nota de orden**: La suscripción de eventos debe configurarse después de que la conexión WebSocket se establezca exitosamente, de lo contrario Feishu reportará error.

## Efecto y Costo

### Datos reales de PoC

| Indicador | Valor |
|------|------|
| Consulta simple ("¿cuánto se vendió hoy?") | ~18K input token + 1.7K output token ≈ **$0.08** |
| Análisis complejo ("tendencia de devoluciones esta semana + análisis de causas") | ~22K input + 3K output ≈ **$0.12** |
| Infraestructura (100 comerciantes comparten) | EKS + EFS (ver sección SaaS abajo) ≈ **$17.74/comerciante/mes** |

### Medios de optimización de costo

- **Prompt Cache (caché de prompts)**: Caché de Bedrock (servicio de modelos base de AWS) puede ahorrar 80-90% de costo de input tokens
- **Modelos por niveles**: Consultas simples usar Claude Haiku (costo aprox. 1/10 de Sonnet), análisis complejos usar Sonnet
- **Objetivo después de optimizar**: Aprox. $45/comerciante/mes (frecuencia de uso media)

### Costo de despliegue SaaS multi-tenant (avanzado)

El PoC de AWS verificó el esquema Per-tenant Pod (grupo de contenedores independiente por inquilino), usando EKS (servicio de contenedores elásticos) + EFS (sistema de archivos elástico) + Envoy Gateway (puerta de enlace de API) para enrutar:

| Escala | Configuración de nodos | Total/mes | Por/comerciante/mes |
|------|---------|---------|-------------|
| 10 comerciantes | 2x r7i.large | ~$190 | ~$19.0 |
| 100 comerciantes | 5x r7i.xlarge | ~$656 | ~$6.6 |
| 1000 comerciantes | 24x r7i.2xlarge | ~$5,671 | ~$5.7 |

> El esquema completo de despliegue K8s (configuración de Envoy Gateway, scripts de gestión de inquilinos, detalles de aislamiento de seguridad) por favor consulta el [artículo original de AWS](https://aws.amazon.com/cn/blogs/china/exploring-openclaw-use-cases-in-ecommerce-platforms/) y [repositorio GitHub](https://github.com/SharonNi/jdopenclaw).

## Consejos prácticos

- **Operaciones de escritura deben tener confirmación manual**: Operaciones como reembolsos, cambio de precios agregan proceso de aprobación en el lado de la API, `safeBins` no puede distinguir lectura/escritura — una vez `curl` entra en la lista blanca, todas las llamadas curl se aprobarán automáticamente
- **Comenzar con solo lectura**: Primero conectar APIs de consulta (ventas, inventario, clientes), después de ejecutar cubrir gradualmente escenarios de operación
- **Aislamiento de personalidad es importante**: Servicio postventa "tono amable, consuelo primero" vs asistente de ventas "datos primero, conclusiones al inicio" — la diferencia en SOUL.md afecta directamente la experiencia del usuario
- **Umbral de escritura de Skill es muy bajo**: Mientras sepas escribir `curl + jq`, puedes encapsular como Skill, no necesita intervención de ingenieros
- **PoC usó Mock API**: El PoC de AWS se basa en datos simulados (12 SKUs, 50 clientes, 20+ endpoints), conectar APIs de plataforma reales necesita adaptar formatos de interfaz por ti mismo
- **Orden de suscripción de eventos de Feishu**: Primero iniciar OpenClaw para establecer conexión WebSocket, luego configurar suscripción de eventos en plataforma abierta de Feishu, de lo contrario reportará error

## Escenarios extendidos (exploración de comunidad)

Los siguientes escenarios tienen discusión y práctica preliminar en la comunidad, pero aún no alcanzan el estándar de verificación completo de extremo a extremo, solo como referencia:

### Monitoreo de precios de competencia

Instalar `browserwing` (Skill de automatización de navegador) + `jd-auto-order` + `im-master` (mensajería cross-plataforma), decir a OpenClaw "monitorear cambios de precios de competencia en JD, notificarme inmediatamente si baja más de 10%" y ejecutar. BrowserWing soporta sitios web nacionales como Taobao/JD, soporta modo de grabación de scripts para reducir consumo de Tokens.

> **Nota**: Usuarios de V2EX reportan que este plugin "no muy estable, frecuentemente se desconecta necesita reconexión manual", se recomienda agregar mecanismo de reconexión en entornos de producción.

### Colaboración multi-rol en e-commerce transfronterizo

Blogueros de comunidad probaron esquemas de 5 empleados digitales de IA: Investigador de mercado VOC (voz del cliente), Optimizador de contenido GEO (optimización geográfica), Mano de plantado en Reddit, Generador de videos de TikTok, Analista de datos, logran división de roles a través de enrutamiento de Feishu. Los detalles de implementación específicos y datos de efecto esperan más verificación independiente.

## Fuente

Proyecto PoC completado por el equipo de AWS China (Ni Huiqing, Huang Xiaoting), usa OpenClaw para construir asistente de vendedor de e-commerce, verifica viabilidad de mecanismo Skill, enrutamiento multi-Agente, notificación activa de Cron y despliegue SaaS multi-tenant. Código PoC es open source en GitHub.

## Enlaces relacionados

- [Blog de AWS China — Exploración de escenarios de aplicación de OpenClaw en plataformas de e-commerce](https://aws.amazon.com/cn/blogs/china/exploring-openclaw-use-cases-in-ecommerce-platforms/) — Artículo completo original
- [Repositorio GitHub jdopenclaw](https://github.com/SharonNi/jdopenclaw) — Código completo del PoC (Mock API, configuración K8s, 4 Skills, scripts de gestión de inquilinos)
- [Plataforma Abierta de Feishu](https://open.feishu.cn/) — Crear aplicación auto-construida empresarial
- [BrowserWing](https://github.com/nicepkg/browserwing) — Skill de automatización de navegador (comunidad)
