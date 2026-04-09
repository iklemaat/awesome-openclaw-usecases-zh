# Limpieza de Bandeja de Entrada

Las suscripciones de newsletters (boletines de noticias) llenan rápidamente la bandeja de entrada. A menudo se acumulan como montañas, nunca se abren.

## Habilidades requeridas
[Configuración de Gmail OAuth](https://clawhub.ai/kai-jar/gmail-oauth).

## Cómo configurar
1. [Opcional] Crear una nueva cuenta de Gmail específicamente para OpenClaw.
2. [Opcional] Cancelar todas las suscripciones de newsletters en tu correo principal, usar el correo de OpenClaw en su lugar.
3. Instalar el skill y asegurar que funcione correctamente.
4. Instruir a OpenClaw:

El siguiente prompt hace que OpenClaw organice automáticamente un resumen de noticias diariamente:

```txt
I want you to run a cron job everyday at 8 p.m. to read all the newsletter emails of the past 24 hours and give me a digest of the most important bits along with links to read more. Then ask for my feedback on whether you picked good bits, and update your memory based on my preferences for better digests in the future jobs.
```

Este prompt requiere que OpenClaw configure una tarea programada (cron job), todos los días a las 8 PM lee todos los correos de suscripción de noticias de las últimas 24 horas, proporciona un resumen del contenido más importante con enlaces para leer más, y luego solicita tu retroalimentación sobre si eligió buenos fragmentos, actualiza su memoria según tus preferencias para generar mejores resúmenes en trabajos futuros.

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/inbox-declutter.md)
