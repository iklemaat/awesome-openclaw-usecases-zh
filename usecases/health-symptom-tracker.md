# Rastreador de Salud y Síntomas

Identificar sensibilidades alimentarias requiere registro continuo a largo plazo, pero este registro es muy tedioso y difícil de mantener. Necesitas recordatorios para registrar, y necesitas análisis para descubrir patrones.

Este flujo de trabajo rastrea automáticamente alimentos y síntomas:

- Enviar información de comidas y síntomas en un tema dedicado de Telegram, OpenClaw registrará automáticamente todo con marcas de tiempo
- 3 recordatorios diarios (mañana, mediodía, noche) para registrar comidas
- Con el tiempo, analiza patrones para identificar posibles desencadenantes

## Habilidades requeridas

- Tarea programada (cron job) para recordatorios
- Tema de Telegram para registro
- Almacenamiento de archivos (archivos de registro markdown)

## Cómo configurar

1. Crear un tema en Telegram llamado "health-tracker" (o similar).
2. Crear archivo de registro: `~/clawd/memory/health-log.md`
3. Indicar a OpenClaw:

El siguiente prompt configura seguimiento automático de alimentos y síntomas con análisis semanal:

```text
When I message in the "health-tracker" topic:
1. Parse the message for food items and symptoms
2. Log to ~/clawd/memory/health-log.md with timestamp
3. Confirm what was logged

Set up 3 daily reminders:
- 8 AM: "Log your breakfast"
- 1 PM: "Log your lunch"
- 7 PM: "Log your dinner and any symptoms"

Every Sunday, analyze the past week's log and identify patterns:
- Which foods correlate with symptoms?
- Are there time-of-day patterns?
- Any clear triggers?

Post the analysis to the health-tracker topic.
```

4. Opcional: Agregar un archivo de memoria a OpenClaw para rastrear desencadenantes conocidos, actualizar continuamente a medida que surgen patrones.

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/health-symptom-tracker.md)
