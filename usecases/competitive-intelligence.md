# Análisis de Competencia y Monitoreo de Precios

> Incluye adaptación local: Índice Baidu / Índice WeChat / Push Feishu / Esquemas alternativos para SerpAPI

Construir un agente de monitoreo de competencia de ejecución continua con Perplexity MCP (búsqueda en línea) y Firecrawl MCP (web scraping). Escanea automáticamente la dinámica de competidores semanalmente, también soporta profundización instantánea por tema en cualquier momento, comprime suscripciones de análisis de competencia de $150+ mensuales y 10 horas de investigación manual a aprox. $1.20 mensuales y 6 minutos.

## Qué puede hacer

- **Modo informe semanal**: Escanea automáticamente lista de competidores predefinidos semanalmente, genera dinámica clave como cambios de precios, actualizaciones de funciones, estrategias de contenido, etc.
- **Modo temático**: Análisis profundo instantáneo para temas específicos (como "lanzamiento de función AI de competidores")
- **Identificación de brechas**: No solo lista datos — automáticamente compara qué cubren competidores, qué omitieron, encuentra tu ventana de oportunidad
- **Salida estructurada**: Devuelve informe de análisis de competencia en formato JSON, incluye citaciones de fuentes, se puede对接 directamente con flujos de trabajo posteriores
- **Costo extremadamente bajo**: Capa gratuita de Perplexity 5 búsquedas diarias, Firecrawl 500 capturas mensuales gratuitas, monitoreo diario básicamente costo cero

## Dolor

Te suscribes a herramientas de análisis de competencia como Semrush, SimilarWeb o Crayon, gastas $150+ mensuales. Pero la mayoría de las funciones no las usas, lo que realmente necesitas es solo "¿qué hicieron los competidores esta semana?". También tienes que gastar varias horas navegando manualmente blogs de competidores, páginas de precios y registros de actualizaciones, organizar hallazgos en informes. Este trabajo es repetitivo, trivial, y una vez que estás ocupado es fácil interrumpir — competidores lanzaron funciones importantes, puedes darte cuenta un mes después.

## Habilidades requeridas

- [Perplexity MCP](https://github.com/ppl-ai/modelcontextprotocol) — Búsqueda en línea, soporta agregación multi-fuente y citaciones (necesita API Key)
- [Firecrawl MCP](https://github.com/mendableai/firecrawl-mcp-server) — Web scraping y extracción estructurada (necesita API Key)

## Cómo configurar

### 1. Obtener claves de API

- Clave de API de Perplexity: Obtener en [perplexity.ai/settings/api](https://perplexity.ai/settings/api) (capa gratuita 5 búsquedas diarias)
- Clave de API de Firecrawl: Registrar en [firecrawl.dev](https://www.firecrawl.dev/) para obtener (capa gratuita 500 capturas mensuales)

### 2. Configurar servidores MCP

```json
{
  "mcpServers": {
    "perplexity": {
      "command": "npx",
      "args": ["-y", "@perplexity-ai/mcp-server"],
      "env": {
        "PERPLEXITY_API_KEY": "${PERPLEXITY_API_KEY}"
      }
    },
    "firecrawl": {
      "command": "npx",
      "args": ["-y", "firecrawl-mcp"],
      "env": {
        "FIRECRAWL_API_KEY": "${FIRECRAWL_API_KEY}"
      }
    }
  }
}
```

> Guardar claves de API en variables de entorno, no hardcodear en archivos de configuración.

### 3. Crear lista de monitoreo de competidores

Crear `.claude/research-profiles/competitor-watchlist.md` en el proyecto, define competidores que quieres monitorear:

```markdown
# Lista de Monitoreo de Competidores

## Competidores Directos (Direct Competitors)
- CompanyA — https://companya.com — Producto principal: XXX
- CompanyB — https://companyb.com — Producto principal: YYY

## Competidores Indirectos (Indirect Competitors)
- CompanyC — https://companyc.com — Ángulo de entrada: ZZZ

## Competidores de Herramientas (Tool Competitors)
- ToolX — https://toolx.dev — Herramientas para desarrolladores

## Puntos clave de monitoreo
- Cambios en página de precios (/pricing)
- Registros de actualizaciones de producto (/changelog, /blog)
- Anuncios de nuevas funciones
- Dinámica de contratación (puede reflejar dirección estratégica)
```

### 4. Agregar instrucciones de agente

Crear comando `/competitive-check`, agregar el siguiente contenido a configuración de OpenClaw:

```text
You are a competitive intelligence analyst. When I say /competitive-check,
do the following:

1. Load the competitor watchlist from .claude/research-profiles/competitor-watchlist.md
2. For each competitor:
   a. Use Perplexity to search for recent news, product updates, and pricing changes
      in the past 7 days
   b. Use Firecrawl to scrape their pricing page and changelog for exact details
3. Compare findings against our product positioning
4. Output a structured report in JSON with these fields:
   - competitor_name
   - changes_detected (array of {type, summary, url, date})
   - pricing_changes (if any)
   - content_gaps (topics they covered that we haven't)
   - recommended_actions (array of suggested responses)
   - sources (array of URLs)

When I say /competitive-check --topic "xxx", focus the analysis on that
specific topic across all competitors instead of a general weekly scan.

Rules:
- Always include source URLs for every claim
- Flag any pricing decrease > 10% as HIGH PRIORITY
- If a competitor launches a feature we don't have, mark it as OPPORTUNITY
- Keep the report actionable — every finding should have a suggested response
```

### 5. Configurar ejecución programada (opcional)

Si necesitas ejecutar automáticamente cada semana, se puede activar a través de la función de tareas programadas integradas de OpenClaw:

```bash
openclaw cron add \
  --name "competitive-weekly" \
  --cron "0 9 * * 1" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "/competitive-check"
```

O directamente configurar en lenguaje natural en el chat:

```text
Ayúdame a crear una tarea programada: ejecutar /competitive-check cada lunes 9 AM, generar informe semanal de competencia.
Configurar zona horaria a Asia/Shanghai, usar sesión aislada.
```

## Casos reales

**Modo informe semanal:**

```
Tú: /competitive-check
```

OpenClaw carga lista de competidores, busca y extrae secuencialmente, devuelve después de aprox. 45 segundos:

> **Informe de Competencia — 2026-03-24**
>
> **CompanyA**
> - [Cambio de precios] Versión profesional bajó de $49/mes a $39/mes (-20%) ⚠️ ALTA PRIORIDAD
> - [Nueva función] Lanzó función de resumen automático con AI
> - Sugerencia: Evaluar si seguir con reducción de precio; función de resumen AI ya está en desarrollo, considerar lanzar anticipadamente
>
> **CompanyB**
> - [Estrategia de contenido] Publicó 3 blogs sobre "seguridad a nivel empresarial"
> - [Contratación] Reclutando 3 ingenieros backend (Go), sospecha de refactorización de infraestructura
> - Sugerencia: Aún no cubrimos tema de seguridad empresarial, se recomienda producir un artículo de referencia
>
> **Brechas de contenido:** Esta semana competidores cubrieron temas "cumplimiento SOC 2" y "residencia de datos", no estamos involucrados.
>
> Fuentes: [12 enlaces de citación]

**Modo temático:**

```
Tú: /competitive-check --topic "estrategia de precios de AI"
```

> Análisis de competencia para estrategia de precios de AI:
>
> - CompanyA: Cobro por token, llamada GPT-4 $0.03/vez
> - CompanyB: Cuota mensual fija + límite de uso, cobro por uso después de exceder
> - CompanyC: Modelo completamente gratis premium, gana con versión empresarial
>
> **Nuestra oportunidad:** Actualmente ningún competidor ofrece modelo de "pago por resultado" (cobro solo por salida exitosa de AI). Este puede ser un ángulo de precios diferenciado.

## Consejos prácticos

- **Primero estrecho luego amplio**: Al inicio solo monitorear 3-5 competidores directos, después de que funcione expandir a competidores indirectos y competidores de herramientas.
- **Página de precios es mina de oro**: Cambios de precios de competidores a menudo reflejan dirección estratégica más que actualizaciones de funciones. Usar Firecrawl para capturar páginas de precios regularmente, comparar con instantáneas históricas.
- **Información de contratación es inteligencia**: Qué personas reclutan competidores, a menudo暗示 dirección de producto en próximos 3-6 meses.
- **Salida对接acciones posteriores**: Conectar informe JSON a webhook de Feishu/Slack, dejar que equipo vea cambios clave第一时间.
- **Controlar costo**: Capa gratuita de Perplexity 5 búsquedas diarias, monitorear 5 competidores justo suficiente. Capa gratuita de Firecrawl 500 capturas mensuales, monitoreo semanal de 10 páginas más que suficiente.

## Adaptación para usuarios de China

### Sustitución de capacidad de búsqueda

Perplexity puede ser inestable al acceder en China. Los siguientes esquemas alternativos pueden lograr capacidad de búsqueda en línea similar:

| Esquema | Escenario aplicable | Descripción |
|------|---------|------|
| [SerpAPI MCP](https://github.com/serpapi/serpapi-mcp-server) | Búsqueda universal | Soporta Baidu, Google y otros motores de búsqueda múltiples, tiene cuota gratuita |
| [Tavily MCP](https://github.com/tavily-ai/tavily-mcp) | Búsqueda optimizada para AI | API de búsqueda diseñada para Agentes de AI, resultados de alta calidad |
| Búsqueda de Baidu MCP | Búsqueda nacional | Hay múltiples implementaciones en comunidad, buscar "baidu mcp server" |

### Complemento de fuentes de datos nacionales

Además del monitoreo estándar de competencia, se recomienda complementar con las siguientes fuentes de datos únicas nacionales:

- **Índice Baidu** (index.baidu.com): Monitorear tendencias de búsqueda de palabras de marca de competidores, cambios de tendencia a menudo significan movimientos mayores
- **Índice WeChat**: Ver en mini-programa de WeChat, refleja cambios de volumen de competidores en ecosistema WeChat
- **巨量算数** (trendinsight.oceanengine.com): Datos de ecosistema Douyin/toutiao, adecuado para monitoreo de competencia de productos C-end
- **Tianyancha / Qichacha**: Monitorear dinámica empresarial de competidores como financiamiento, cambios de personal, solicitudes de patentes, etc.
- **36Kr / IT Juzi**: Noticias de la industria y datos de financiamiento

### Adaptación de canal de push

Enviar informe semanal de competencia a herramientas de colaboración de equipo comúnmente usadas nacionalmente:

```text
After completing the competitive analysis, send the report summary
to our team channel:
- Format the key findings as a structured message
- Include HIGH PRIORITY items at the top
- Add source links for verification
- Use Feishu/Lark webhook: ${FEISHU_WEBHOOK_URL}
```

Ejemplo de push de webhook de Feishu:

```bash
# Enviar informe semanal de competencia vía bot personalizado de Feishu
curl -X POST "${FEISHU_WEBHOOK_URL}" \
  -H "Content-Type: application/json" \
  -d '{
    "msg_type": "interactive",
    "card": {
      "header": {
        "title": {"tag": "plain_text", "content": "Informe de Competencia - 2026-03-24"},
        "template": "orange"
      },
      "elements": [
        {
          "tag": "markdown",
          "content": "**⚠️ CompanyA redujo precio de versión profesional 20%**\nVer detalles en informe completo"
        }
      ]
    }
  }'
```

> Usuarios de DingTalk reemplazar con URL de webhook de DingTalk, formato de mensaje similar. WeCom similarmente.

### Estado actual de herramientas de monitoreo de competencia nacionales

Actualmente las herramientas de monitoreo de competencia nacionales (como鹰眼监测，新榜等) se enfocan principalmente en monitoreo de opinión pública y contenido, capacidades limitadas en comparación de funciones de producto y seguimiento de precios. La ventaja del esquema con OpenClaw + MCP es:

- **Altamente personalizable**: Tú decides qué monitorear, cómo analizar, qué formato输出
- **Costo controlable**: Capa gratuita cubre necesidades de equipos pequeños, no necesita suscripción de SaaS costoso
- **Expandible**: Fácil接入 nuevas fuentes de datos, no limitado por funciones de plataforma

## Diferencia con "Investigación de Mercados y Fábrica de Productos"

| Dimensión | Investigación de Mercados y Fábrica de Productos | Análisis de Competencia y Monitoreo de Precios |
|------|-------------------|----------------------|
| Objetivo | Descubrir oportunidades de producto desde cero y construir MVP | Seguimiento continuo de competidores conocidos |
| Frecuencia | Ejecución única u ocasional | Ejecución automática semanal, ejecución continua |
| Fuente de datos | Puntos dolorosos de usuarios en Reddit, X | Sitio web oficial de competidores, página de precios, registro de actualizaciones |
| Salida | Idea de producto + código MVP | Informe estructurado de inteligencia de competencia |
| Etapa aplicable | Producto de 0 a 1 | Competencia continua después de lanzamiento de producto |

## Enlaces relacionados

- [Servidor MCP de Perplexity](https://github.com/ppl-ai/modelcontextprotocol)
- [Servidor MCP de Firecrawl](https://github.com/mendableai/firecrawl-mcp-server)
- [Agente de Inteligencia de Competencia de SerpAPI](https://github.com/serpapi/competitive-intelligence-agent) — Implementación de referencia de Agente de análisis de competencia oficial de SerpAPI
- [MCP de Agente de Re-precios de Competencia](https://github.com/ajeetraina/competitive-repricing-agent-mcp) — Ejemplo de MCP de monitoreo de precios e informe automático

---

**Enlace original**: [El Agente de AI de Claude que Reemplazó mi Herramienta de Análisis de Competencia de $150/Mes](https://genaiunplugged.substack.com/p/competitive-ai-analysis-agent)
