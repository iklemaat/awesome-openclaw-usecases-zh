# Panel Dinámico y Generación con Sub-Agentes

Los paneles estáticos muestran datos obsoletos y requieren actualizaciones manuales continuas. Necesitas visibilidad en tiempo real a través de múltiples fuentes de datos, sin construir un frontend personalizado ni activar límites de tasa de API.

Este flujo de trabajo crea un panel en tiempo real, genera sub-agentes (sub-agent) para obtener y procesar datos en paralelo:

- Monitorea múltiples fuentes de datos simultáneamente (API, base de datos, GitHub, redes sociales)
- Genera un sub-agente por cada fuente de datos, evita bloqueos y distribuye carga de API
- Agrega resultados a un panel unificado (texto, HTML o Canvas)
- Actualiza cada N minutos con los datos más recientes
- Envía alertas cuando las métricas exceden umbrales
- Mantiene tendencias históricas en base de datos para visualización

## Dolor

Construir paneles personalizados toma semanas. Para cuando terminas, los requisitos han cambiado. Sondear múltiples APIs secuencialmente es lento y fácil de tocar límites de tasa. Necesitas información ahora, no gastar un fin de semana escribiendo código.

## Qué puede hacer

Defines qué rastrear vía conversación: "Rastrea estrellas de GitHub, menciones en Twitter, volumen de trading de Polymarket y salud del sistema." OpenClaw genera sub-agentes para obtener cada fuente de datos en paralelo, agrega resultados, y envía un panel formateado a Discord o genera un archivo HTML. Las actualizaciones se ejecutan automáticamente vía tareas programadas (cron jobs).

Ejemplos de secciones del panel:
- **GitHub**: Estrellas, Forks, Issues no cerrados, commits recientes
- **Redes sociales**: Menciones en Twitter, discusiones en Reddit, actividad en Discord
- **Mercados**: Volumen de Polymarket, tendencias de predicción
- **Salud del sistema**: Uso de CPU, memoria, disco, estado de servicios

## Habilidades requeridas

- Generación de sub-agentes para ejecución paralela
- `github` (CLI gh) para métricas de GitHub
- `bird` (Twitter) para datos sociales
- `web_search` o `web_fetch` para APIs externas
- `postgres` para almacenar métricas históricas
- Discord o Canvas para renderizar el panel
- Tareas programadas (cron job) para actualizaciones periódicas

## Cómo configurar

1. Configurar base de datos de métricas:
```sql
-- Tabla de métricas
CREATE TABLE metrics (
  id SERIAL PRIMARY KEY,
  source TEXT, -- por ejemplo "github", "twitter", "polymarket"
  metric_name TEXT,
  metric_value NUMERIC,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla de alertas
CREATE TABLE alerts (
  id SERIAL PRIMARY KEY,
  source TEXT,
  condition TEXT,
  threshold NUMERIC,
  last_triggered TIMESTAMPTZ
);
```

2. Crear un canal de Discord para actualizaciones del panel (por ejemplo #dashboard).

3. Configurar prompt para OpenClaw:

El siguiente prompt hace que el agente obtenga datos multi-fuente cada 15 minutos y genere un panel:

```text
You are my dynamic dashboard manager. Every 15 minutes, run a cron job to:

1. Spawn sub-agents in parallel to fetch data from:
   - GitHub: stars, forks, open issues, commits (past 24h)
   - Twitter: mentions of "@username", sentiment analysis
   - Polymarket: volume for tracked markets
   - System: CPU, memory, disk usage via shell commands

2. Each sub-agent writes results to the metrics database.

3. Aggregate all results and format a dashboard:

📊 **Dashboard Update** — [timestamp]

**GitHub**
- ⭐ Stars: [count] (+[change])
- 🍴 Forks: [count]
- 🐛 Open Issues: [count]
- 💻 Commits (24h): [count]

**Social Media**
- 🐦 Twitter Mentions: [count]
- 📈 Sentiment: [positive/negative/neutral]

**Markets**
- 📊 Polymarket Volume: $[amount]
- 🔥 Trending: [market names]

**System Health**
- 💻 CPU: [usage]%
- 🧠 Memory: [usage]%
- 💾 Disk: [usage]%

4. Post to Discord #dashboard.

5. Check alert conditions:
   - If GitHub stars change > 50 in 1 hour → ping me
   - If system CPU > 90% → alert
   - If negative sentiment spike on Twitter → notify

Store all metrics in the database for historical analysis.
```

4. Opcional: Usar Canvas para renderizar un panel HTML con gráficos.

5. Consultar datos históricos: "Muestra el crecimiento de estrellas de GitHub de los últimos 30 días."

## Enlaces relacionados

- [Procesamiento paralelo con sub-agentes](https://docs.openclaw.ai/subagents)
- [Principios de diseño de paneles](https://www.nngroup.com/articles/dashboard-design/)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/dynamic-dashboard.md)
