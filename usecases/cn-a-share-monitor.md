# Monitor Diario de Mercado de Acciones A

> Complementario con [Rastreador de Informes Financieros IA](earnings-tracker.md): El rastreador de informes se enfoca en eventos trimestrales de empresas individuales, este caso se enfoca en la dinámica global del mercado cada día de trading.

Revisar reportes antes de la apertura, monitorear software de cotizaciones durante el trading, revisar después del cierre — los inversores individuales gastan mucho más tiempo recopilando información que tomando decisiones. Si la IA puede organizar diariamente la tendencia del mercado, movimientos de sectores y flujos de capital, solo necesitas ver las conclusiones.

Este caso de uso hace que OpenClaw recolecte automáticamente datos de cotizaciones de acciones A cada día de trading, y los organice en un resumen estructurado para enviarte.

## Qué puede hacer

- **Resumen pre-apertura**: Tendencias de mercados externos overnight, recordatorios de eventos importantes del día (como ajustes LPR, suscripción IPO)
- **Revisión post-cierre**: subidas/bajadas de índices principales, comparación de volumen de trading, estadísticas de empresas al alza/baja
- **Seguimiento de sectores**: Clasificación de subidas/bajas del día por sector industrial, identificar rotación de sectores calientes
- **Flujo de capital**: TOP de entradas/salidas netas de capital principal, dinámica de capital del norte
- **Monitoreo de selección propia**: Rendimiento del día de tus acciones seguidas, cambios en indicadores técnicos

## Límites de capacidad (basado en pruebas de la comunidad)

Según retroalimentación de uso real de múltiples desarrolladores, **la recopilación de información y organización de datos es la capacidad más confiable de OpenClaw en el escenario de acciones A**. Los siguientes son los límites de capacidad verificados con pruebas reales:

| Escenario | Confiabilidad | Descripción |
|------|--------|------|
| Recopilación de datos de cotizaciones | ✅ Confiable | Interfaz AKShare estable, cobertura de datos completa |
| Organización y envío de datos | ✅ Confiable | Salida estructurada + envío cron programado |
| Cálculo de indicadores técnicos | ✅ Confiable | MA/MACD/RSI etc. basados en fórmulas, resultados determinísticos |
| Agregación de opinión pública/noticias | ⚠️ Básicamente usable | Depende de automatización del navegador, ocasionalmente tiene límites anti-scraping |
| Sugerencias de compra/venta de IA | ❌ No recomendable depender | Llamadas LLM tienen retraso de 2-5 segundos, conexión Chrome inestable, caja negra de decisiones |

> Fuente de citación: [Jason Zhang - ¿Usar OpenClaw para cuantificar acciones A? Lo probé, hablemos de sensaciones reales](https://junxinzhang.com/openclaw-quant-trading-a-share/) (12 años experiencia IT, desarrollador con 3 años experiencia en cuantificación de acciones de EE.UU. probó)

## Habilidades requeridas

**Capa de datos** (dependencia principal):

[AKShare](https://github.com/akfamily/akshare) (16,000+ stars, MIT open source) — La biblioteca de facto estándar de datos financieros de China, cubre acciones A Shanghai-Shenzhen, acciones H, ETF, bonos convertibles, indicadores macroeconómicos. Gratis, no necesita API Key.

Plan MCP opcional (evita escribir scripts Python):

- [mcp-cn-a-stock](https://github.com/elsejj/mcp-cn-a-stock) (400+ stars) — Servicio MCP de datos de acciones A diseñado específicamente para LLM, proporciona consulta de información básica, cotizaciones, finanzas e indicadores técnicos de acciones individuales (tres herramientas brief/medium/full), adecuado para análisis profundo de acciones individuales. Resumen del mercado, clasificación de sectores, flujo de capital y otros datos globales aún necesitan obtenerse vía AKShare

**Capa de envío**:

Feishu / DingTalk / WeCom (referir casos de uso de integración IM correspondientes)

## Cómo configurar

### Plan Uno: Servicio MCP + consulta en lenguaje natural (recomendado)

Sin escribir código, dejar que OpenClaw obtenga directamente datos de acciones A a través del servicio MCP. Este proyecto proporciona una dirección de servicio público, no necesita instalación local:

Agregar servidor MCP en la configuración de OpenClaw (usar servicio público HTTP):

```json
{
  "mcpServers": {
    "cn-a-stock": {
      "url": "http://82.156.17.205/cnstock/mcp"
    }
  }
}
```

> **Nota**: Este es un servicio de demostración público proporcionado por el autor del proyecto, la dirección IP puede cambiar o salir de línea. Para producción se recomienda auto-construir el servicio.
>
> **Despliegue local**: Este proyecto depende de Python ≥ 3.12 y biblioteca C ta-lib, ejecutar mediante `python main.py --transport=http` iniciar. Ver [README del proyecto](https://github.com/elsejj/mcp-cn-a-stock) para detalles.

Luego usar directamente lenguaje natural para consultar información de acciones individuales:

```text
Ayúdame a ver las últimas cotizaciones e indicadores técnicos de Midea Group SZ000333.

¿Cómo están los datos financieros recientes de Kweichow Moutai SH600519?
```

> **Notas**:
> - El servicio MCP proporciona tres herramientas brief/medium/full, enfocado en consulta de información básica, cotizaciones, finanzas e indicadores técnicos de **acciones A individuales**. Índices del mercado, clasificación de sectores, capital del norte y otros datos globales necesitan usarse junto con el Plan Dos (AKShare)
> - Los códigos de acciones deben usar formato con prefijo `SH`/`SZ` (como `SH600519`, `SZ000333`), LLM generalmente puede inferir y convertir automáticamente
> - **Solo soporta acciones A**, acciones H (Xiaomi, Tencent, Alibaba, etc.) no están en la cobertura

### Plan Dos: AKShare + sandbox Python

Adecuado para usuarios que quieren control más flexible sobre la lógica de procesamiento de datos:

```bash
pip install akshare
```

Luego dejar que OpenClaw llame AKShare en el sandbox Python:

```text
Usa akshare para obtener las 10 acciones con mayor subida en acciones A hoy,
mostrar nombre de la acción, código, porcentaje de subida, volumen de trading, tasa de rotación.
```

Consulta rápida de interfaces comunes:

| Interface | Uso |
|------|------|
| `ak.stock_zh_a_spot_em()` | Cotizaciones en tiempo real de todas las acciones A ⚠️ |
| `ak.stock_zh_a_hist()` | Datos históricos diarios de acciones individuales |
| `ak.stock_zh_index_daily_em()` | Cotizaciones diarias históricas de índices principales |
| `ak.stock_hk_hist()` | Historial de cotizaciones de acciones H individuales |
| `ak.stock_board_industry_name_em()` | Lista de sectores industriales ⚠️ |
| `ak.stock_hsgt_fund_flow_summary_em()` | Resumen de flujo de capital de Shanghai-Hong Kong Connect |
| `ak.stock_market_activity_legu()` | Estadísticas de empresas al alza/baja/límite arriba/abajo |

> ⚠️ Las interfaces marcadas dependen de East Money en la capa inferior, las llamadas desde IPs extranjeras pueden ser rechazadas. Plan alternativo: cotizaciones en tiempo real se pueden usar `stock_zh_a_hist` para tomar datos del día, índices se pueden usar `stock_zh_index_daily_em`.

### Configurar envío automático diario

Configurar tareas programadas, enviar automáticamente resumen pre-apertura y revisión post-cierre en días de trading:

```text
Ayúdame a crear dos tareas programadas:

1. Resumen pre-apertura: Cada día de trading a las 8:30 AM (Asia/Shanghai),
   organizar subidas/bajas de los tres índices principales de EE.UU. overnight, tendencia de futuros A50, calendario de eventos importantes del día,
   enviar a Feishu.

2. Revisión post-cierre: Cada día de trading a las 3:30 PM (Asia/Shanghai),
   organizar subidas/bajas del mercado principal hoy, cambio en volumen de trading, TOP 5 de subidas/bajas de sectores,
   flujo de capital del norte, rendimiento de mis acciones seleccionadas,
   enviar a Feishu.

Lista de acciones seleccionadas: 600519 Kweichow Moutai, 300750 CATL, 000858 Wuliangye.
```

## Consejos prácticos

- **AKShare es la fuente de datos preferida**: Gratis, open source, interfaces completas. Si necesitas datos de mayor frecuencia (nivel minuto), puedes solicitar puntos Tushare Pro (pasar mediante variable de entorno `TUSHARE_TOKEN`)
- **Nota para servidores extranjeros**: Algunas interfaces de AKShare (como `stock_zh_a_spot_em`, `stock_zh_index_spot_em`, `stock_board_industry_name_em`) dependen de East Money en la capa inferior, las llamadas desde IPs extranjeras serán rechazadas. Plan alternativo: acciones individuales usar `stock_zh_a_hist`, índices usar `stock_zh_index_daily_em`, acciones H usar `stock_hk_hist`. O desplegar directamente en servidores en la nube de China
- **Cobertura de acciones H**: Xiaomi, Alibaba, Tencent y otras acciones H no están en la cobertura del servicio MCP, necesitan obtenerse a través de interfaces como `stock_hk_hist(symbol="01810")` de AKShare
- **Plan MCP es más sencillo**: No necesitas escribir Python tú mismo, OpenClaw llama automáticamente las interfaces de datos a través del protocolo MCP. Nota que el servicio MCP se enfoca en consultas de acciones individuales, datos globales (mercado, sectores, flujo de capital) aún necesitan AKShare
- **Despliegue en la nube para mantener en línea**: Si necesitas enviar puntualmente cada día, se recomienda desplegar en servidor en la nube (servidor ligero Alibaba Cloud/Tencent Cloud, costo mensual de decenas de yuanes),配合 cron para lograr completamente automático
- **No depender de IA para decisiones de trading**: Los LLM actuales tienen problemas de retraso, inestabilidad e inexplicabilidad en decisiones financieras. Usar OpenClaw como herramienta de recopilación y organización de información, dejar las decisiones para ti
- **Notar frecuencia de interfaces**: AKShare depende de datos de sitios web públicos en la capa inferior, las llamadas de alta frecuencia pueden ser limitadas en velocidad. Monitoreo diario (varias veces al día) no tiene problema

> **⚠️ Descargo de responsabilidad**: Este caso de uso es solo para organización de información de inversión personal, no constituye consejo de inversión. El mercado de valores tiene riesgos, invertir con precaución.

## Enlaces relacionados

- [Documentación oficial de AKShare](https://akshare.akfamily.xyz/) — Interfaces de datos de acciones A gratis y open source (16,000+ stars)
- [mcp-cn-a-stock - GitHub](https://github.com/elsejj/mcp-cn-a-stock) — Servicio MCP de datos de acciones A (400+ stars)
- [Jason Zhang - Sensaciones reales de usar OpenClaw para cuantificar acciones A](https://junxinzhang.com/openclaw-quant-trading-a-share/) — Reporte de prueba más honesto
- [Tencent Cloud - Despliegue Lighthouse de analista de mercado de valores OpenClaw](https://blog.csdn.net/u012263509/article/details/157762036) — Tutorial de despliegue
- [Alibaba Cloud - Despliegue sin barrera de OpenClaw accediendo a datos de acciones A](https://developer.aliyun.com/article/1713167) — Tutorial de despliegue
