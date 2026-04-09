# Rastreador de Informes Financieros Impulsado por IA

> Incluye adaptación local: AKShare / East Money / CNINFO

Seguimiento de docenas de empresas tecnológicas en temporada de informes financieros significa necesitar verificar múltiples fuentes de información y recordar fechas de reportes. Quieres mantenerte al día con la dinámica de informes financieros de empresas de IA/tecnología, sin tener que rastrear manualmente cada empresa.

Este flujo de trabajo automatiza el seguimiento y push de informes financieros:

- Vista previa del domingo: Escanea el calendario próximo de informes financieros, publica empresas de IA/tecnología relevantes a Telegram
- Tú eliges qué empresas seguir, OpenClaw programa un cron job de un solo uso para cada fecha de informe
- Después de que cada reporte se publica, OpenClaw busca resultados, formatea resumen detallado (superó/no superó expectativas, indicadores clave, aspectos destacados de IA), y te envía

## Habilidades requeridas

- `web_search` (integrado)
- Soporte de cron job (tarea programada) de OpenClaw
- Tema de Telegram para actualizaciones de informes financieros

## Cómo configurar

1. Crear un tema de Telegram llamado "earnings" para recibir actualizaciones.
2. Enviar el siguiente prompt a OpenClaw:
```text
Every Sunday at 6 PM, run a cron job to:
1. Search for the upcoming week's earnings calendar for tech and AI companies
2. Filter for companies I care about (NVDA, MSFT, GOOGL, META, AMZN, TSLA, AMD, etc.)
3. Post the list to my Telegram "earnings" topic
4. Wait for me to confirm which ones I want to track

When I reply with which companies to track:
1. Schedule one-shot cron jobs for each earnings date/time
2. After each report drops, search for earnings results
3. Format a summary including: beat/miss, revenue, EPS, key metrics, AI-related highlights, guidance
4. Post to Telegram "earnings" topic

Keep a memory of which companies I typically track so you can auto-suggest them each week.
```

## Adaptación para usuarios de China

El mercado de acciones A tiene un sistema completo de divulgación de informes financieros, y la obtención de datos es más estandarizada que las acciones de EE.UU. Los siguientes son esquemas de adaptación para inversores de acciones A.

### Diferencias de mecanismo de informes financieros entre acciones A y acciones de EE.UU.

| Dimensión | Acciones de EE.UU. | Acciones A |
|------|------|------|
| Calendario de informes | Empresas publican por sí mismas, necesita depender de agregación de terceros | La bolsa requiere publicar **calendario de divulgación programada** por adelantado, más estandarizado |
| Ritmo de publicación | Publicación trimestral única | **Mecanismo de tres etapas**: pronóstico de resultados → boletín de resultados → informe formal |
| Obtención de datos | Necesita API de pago (Alpha Vantage, etc.) | Hay esquemas open source gratis (AKShare) |

Esto significa que el seguimiento de informes financieros de acciones A se puede hacer más fino — puedes obtener señales anticipadas en la etapa de pronóstico de resultados.

### Fuentes de datos de acciones A recomendadas

| Fuente de datos | Costo | Descripción |
|--------|------|------|
| **AKShare** | Gratis | Recomendado como primera opción, open source MIT, interfaces más completas |
| Tushare Pro | Sistema de puntos | Básico gratis, funciones avanzadas necesitan puntos |
| East Money Choice | De pago | Nivel profesional, adecuado para usuarios institucionales |
| CNINFO (巨潮资讯) | Gratis | Canal de divulgación oficial, adecuado para obtener PDF de anuncios originales |

**Esquema recomendado: AKShare** ([GitHub](https://github.com/akfamily/akshare), 10k+ stars, licencia MIT)

Instalación:

```bash
pip install akshare
```

Interfaces clave:

| Interface | Uso | Correspondiente a función de caso de uso original |
|------|------|---------------|
| `stock_yysj_em(date)` | Calendario de divulgación programada | earnings calendar (núcleo) |
| `stock_yjyg_em()` | Datos de pronóstico de resultados | Señal anticipada, acciones de EE.UU. no tienen correspondiente |
| `stock_yjkb_em()` | Boletín de resultados | Resumen rápido de resultados |
| `stock_yjbb_em()` | Informe formal de resultados | Datos completos de informe financiero |

Ejemplo de inicio rápido:

```python
import akshare as ak

# Obtener calendario de divulgación programada de informe trimestral 2025
df = ak.stock_yysj_em(date="20250331")
# Filtrar empresas que te importan (filtrar por código de acción)
watchlist = ["600519", "000858", "601318", "000001"]
my_stocks = df[df["股票代码"].isin(watchlist)]
print(my_stocks[["股票代码", "股票简称", "首次预约时间"]])
```

### Adaptación de canal de push

| Esquema original | Reemplazo nacional | Descripción |
|---------|---------|------|
| Telegram | **Bot de grupo de DingTalk** | Forma Webhook, más simple |
| Telegram | **Bot de Feishu** | Soporta mensajes de tarjeta de texto enriquecido |
| Telegram | **Aplicación de WeCom** | Primera opción para usuarios empresariales |

### Adaptación de prompt

Reemplazar parte de acciones de EE.UU. en prompt original con lógica de acciones A:

```text
Cada domingo 8 PM, ejecutar tarea programada:
1. Llamar interface stock_yysj_em de AKShare, obtener calendario de divulgación programada de la próxima semana
2. Filtrar empresas que sigo (600519 Kweichow Moutai, 000858 Wuliangye, 601318 Ping An, etc.)
3. Push lista a grupo de DingTalk "Seguimiento de Informes Financieros"
4. Esperar a que confirme cuáles quiero rastrear

Después de que confirmo:
1. Programar tareas programadas de un solo uso para cada fecha de divulgación
2. Después de que se publica el reporte, buscar boletín de resultados e interpretación del mercado
3. Formatear resumen: ingresos, beneficio neto, crecimiento interanual, comparación de pronóstico de resultados, comentarios de instituciones
4. Push a grupo de DingTalk

Recordar lista de empresas que normalmente sigo, recomendar automáticamente cada semana.
Monitorear simultáneamente pronósticos de resultados (stock_yjyg_em), notificarme anticipadamente cuando haya lanzamiento de pronóstico.
```

### Recordatorio de cumplimiento

- Datos solo para estudio e investigación personal, no constituye consejo de inversión
- Controlar frecuencia de llamadas a API, evitar ejercer presión sobre plataformas upstream
- AKShare depende de datos de sitios web públicos en la capa inferior, interfaces pueden necesitar actualización debido a cambios upstream, se recomienda seguir sus [GitHub Issues](https://github.com/akfamily/akshare/issues)

### Enlaces relacionados

- [Documentación de AKShare](https://akshare.akfamily.xyz/) — Interfaces de datos de acciones A gratis y open source
- [CNINFO](https://www.cninfo.com.cn/) — Plataforma oficial de divulgación de información
- [Centro de datos de East Money](https://data.eastmoney.com/) — Calendario de informes financieros y consulta de datos

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/earnings-tracker.md)
