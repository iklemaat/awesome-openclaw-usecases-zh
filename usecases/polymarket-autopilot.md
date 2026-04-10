# Piloto Automático de Polymarket: Trading Simulado Automatizado

Monitorear manualmente oportunidades de arbitraje en mercados de predicción y ejecutar trades consume mucho tiempo y requiere atención continua. Quieres probar y optimizar estrategias de trading sin arriesgar dinero real.

Este flujo de trabajo usa estrategias personalizadas para automatizar el trading simulado (paper trading) en Polymarket:

- Monitorea datos de mercado vía API (precios, volumen, diferenciales)
- Ejecuta trades simulados usando estrategias TAIL (seguimiento de tendencias) y BONDING (contrarias)
- Rastrea rendimiento de portafolio, ganancias/pérdidas y tasa de victorias
- Envía resúmenes diarios a Discord con registros de trades e insights
- Aprende de patrones: ajusta parámetros de estrategia basado en resultados de backtesting

## Dolor

Los mercados de predicción cambian rápidamente. El trading manual significa perder oportunidades, decisiones emocionales y dificultad para rastrear qué estrategias funcionan. Probar estrategias con dinero real antes de entender el comportamiento del mercado conlleva riesgo de pérdidas.

## Qué puede hacer

El sistema de piloto automático escanea continuamente oportunidades en Polymarket, ejecuta trades simulados usando estrategias configurables, y registra todo para análisis. Te despiertas y ves un resumen de sus "trades overnight" — qué funcionó, qué no.

Ejemplos de estrategias:
- **TAIL**: Sigue tendencias fuertes cuando el volumen se dispara
- **BONDING**: Compra posiciones contrarias cuando el mercado reacciona en exceso a noticias
- **SPREAD**: Identifica mercados con desviaciones de precios, busca oportunidades de arbitraje

## Habilidades requeridas

- `web_search` o `web_fetch` (para obtener datos de API de Polymarket)
- `postgres` o SQLite, para registro de trades y seguimiento de portafolio
- Integración con Discord, para reportes diarios
- Cron job (tarea programada), para monitoreo continuo
- Generación de Sub-agent (sub-agente), para análisis de mercado paralelo

## Cómo configurar

1. Configurar base de datos para trading simulado:
```sql
CREATE TABLE paper_trades (
  id SERIAL PRIMARY KEY,
  market_id TEXT,
  market_name TEXT,
  strategy TEXT,
  direction TEXT,
  entry_price DECIMAL,
  exit_price DECIMAL,
  quantity DECIMAL,
  pnl DECIMAL,
  timestamp TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE portfolio (
  id SERIAL PRIMARY KEY,
  total_value DECIMAL,
  cash DECIMAL,
  positions JSONB,
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

2. Crear un canal de Discord para actualizaciones (por ejemplo #polymarket-autopilot).

3. Enviar el siguiente prompt a OpenClaw:
```text
You are a Polymarket paper trading autopilot. Run continuously (via cron every 15 minutes):

1. Fetch current market data from Polymarket API
2. Analyze opportunities using these strategies:
   - TAIL: Follow strong trends (>60% probability + volume spike)
   - BONDING: Contrarian plays on overreactions (sudden drops >10% on news)
   - SPREAD: Arbitrage when YES+NO > 1.05
3. Execute paper trades in the database (starting capital: $10,000)
4. Track portfolio state and update positions

Every morning at 8 AM, post a summary to Discord #polymarket-autopilot:
- Yesterday's trades (entry/exit prices, P&L)
- Current portfolio value and open positions
- Win rate and strategy performance
- Market insights and recommendations

Use sub-agents to analyze multiple markets in parallel during high-volume periods.

Never use real money. This is paper trading only.
```

4. Iterar estrategias basado en rendimiento. Ajustar umbrales, agregar nuevas estrategias, hacer backtesting en datos históricos.

## Enlaces relacionados

- [API de Polymarket](https://docs.polymarket.com/)
- [Mejores prácticas de trading simulado](https://www.investopedia.com/articles/trading/11/paper-trading.asp)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/polymarket-autopilot.md)
