# Resumen Matutino Personalizado

Los primeros 30 minutos después de despertar cada día los pasas alcanzando información — revisar noticias, consultar calendario, verificar tareas pendientes, tratando de descubrir qué es lo más importante hoy. ¿Y si todo esto ya estuviera completado, esperándote en un mensaje?

Este flujo de trabajo hace que OpenClaw te envíe un resumen matutino completamente personalizado a una hora programada cada día, cubriendo noticias, tareas, ideas creativas y sugerencias activas.

## Qué puede hacer

- Enviar un reporte matutino estructurado a Telegram, Discord o iMessage cada día a una hora fija (por ejemplo, 8:00 AM)
- Investigar noticias overnight relacionadas con tus intereses navegando la web
- Revisar tu lista de tareas y presentar las tareas del día
- Generar resultados creativos mientras duermes (scripts completos, borradores de correos, propuestas de negocios — no solo ideas)
- Recomendar tareas que la IA puede completar autónomamente por ti ese día

## Dolor

Gastas tu tiempo matutino más productivo en ponerte al día. Mientras tanto, tu agente de IA está inactivo toda la noche. El resumen matutino convierte el tiempo nocturno inactivo en tiempo de preparación eficiente — el trabajo ya está completado cuando despiertas.

## Habilidades requeridas

- Integración con Telegram, Discord o iMessage
- Integración con Todoist / Apple Reminders / Asana (la herramienta de gestión de tareas que uses)
- [x-research-v2](https://clawhub.ai) para investigación de tendencias en redes sociales (opcional)

## Cómo configurar

1. Conecta OpenClaw a tu plataforma de mensajería y gestor de tareas.

2. Configura el prompt para OpenClaw:

El siguiente prompt hace que el agente genere y envíe automáticamente un resumen matutino personalizado cada mañana:

```text
Quiero configurar un resumen matutino regular. Cada mañana a las 8:00 AM,
envíame un reporte a través de Telegram.

Quiero que este reporte incluya:
1. Noticias relevantes para mis intereses (IA, startups, tecnología)
2. Ideas para contenido que puedo crear hoy
3. Tareas que necesito completar hoy (extraer de mi lista de tareas)
4. Recomendaciones de tareas que puedes completar por mí hoy

Para las ideas de contenido, escribe borradores de scripts/esquemas completos — no solo títulos.
```

3. OpenClaw programará automáticamente la tarea programada. Revisa tu mensaje la mañana siguiente para verificar que funciona correctamente.

4. Personaliza con el tiempo — simplemente envía un mensaje a tu bot:

Aquí hay algunos ejemplos de instrucciones para personalizar el contenido del resumen:

```text
Agrega el pronóstico del tiempo a mi resumen matutino.
Deja de incluir noticias generales, enfócate solo en IA.
Incluye una frase motivacional cada mañana.
```

5. Si no se te ocurre qué contenido incluir, no es necesario forzarte — simplemente di:

El siguiente prompt deja que el agente decida autónomamente qué contenido es más útil en el resumen:

```text
Quiero que este reporte incluya cosas relevantes para mí.
Piensa qué sería más útil poner en este reporte.
```

## Ideas clave

- La sección de tareas recomendadas por la IA es la función más poderosa — hace que el agente piense activamente cómo ayudarte, en lugar de esperar comandos.
- Puedes personalizar el resumen enviando mensajes. Di "agrega precios de acciones a mi resumen matutino", y se actualizará.
- Los borradores completos (no solo ideas) son clave para ahorrar tiempo. Despiertas y ves el script, no sugerencias.
- No importa en qué industria estés — un resumen matutino que incluye tareas, noticias y recomendaciones activas es universalmente útil.

## Adaptación para usuarios en España/Latam

Si usas otras plataformas de mensajería, solo necesitas reemplazar el canal de envío y las fuentes de noticias, la lógica es exactamente la misma.

### Reemplazo del canal de envío

Reemplaza Telegram / Discord en el prompt con el IM que realmente uses:

```text
Quiero configurar un resumen matutino regular. Cada mañana a las 8:00 AM,
envíame un reporte a través de WhatsApp / Slack / Email.
```

Siempre que hayas configurado el canal IM correspondiente (referir [Asistente IA de Feishu](cn-feishu-ai-assistant.md) o integración de IM similar).

### Fuentes de noticias recomendadas en español

Reemplaza las fuentes de noticias en inglés con fuentes en español, para que el resumen se adapte mejor al entorno de información local:

```text
Quiero configurar un resumen diario matutino. Cada mañana a las 8:00 AM envíame un reporte.

Contenido:
1. Noticias overnight relevantes para mí (seguir dirección IA, emprendimiento, tecnología, priorizar El País, Expansión, TechCrunch en español, Hacker News en español)
2. Tareas que necesito completar hoy (extraer de mi lista de tareas)
3. Cosas que sugieres que puedes ayudarme a completar automáticamente hoy

Para la sección de ideas de contenido, escribe borradores completos directamente, no solo listes títulos.
```

> **Consejo**: Para hacer scraping de sitios en español como El País, Expansión, etc., OpenClaw necesita capacidad de navegación web o búsqueda. Si tu agente no puede acceder a estos sitios, confirma que tiene instalada la skill de navegador o skill de búsqueda web.

### Configuración de tarea programada (cron)

OpenClaw soporta crear tareas programadas usando lenguaje natural:

```text
Ayúdame a crear una tarea programada: cada día laboral a las 7:30 AM (30 minutos antes que la versión original de 8:00, adaptado al horario laboral local), generar resumen matutino y enviar a mi canal.
Configurar zona horaria a Europe/Madrid o America/Mexico_City según corresponda.
```

También se puede controlar precisamente con línea de comandos:

```bash
openclaw cron add \
  --name "resumen-matutino-es" \
  --cron "30 7 * * 1-5" \
  --tz "Europe/Madrid" \
  --session isolated \
  --message "Generar resumen matutino de hoy y enviar a mi canal" \
  --announce \
  --channel telegram
```

## Fuentes de referencia

Inspirado en [video de Alex Finn sobre casos de uso de OpenClaw que cambian la vida](https://www.youtube.com/watch?v=41_TNGDDnfQ).
