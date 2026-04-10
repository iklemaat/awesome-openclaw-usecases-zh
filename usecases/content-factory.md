# Fábrica de Contenido Multi-Agente

Eres un creador de contenido, necesitas equilibrar investigación, escritura y diseño en múltiples plataformas. Cada paso — encontrar temas populares, escribir guiones, generar miniaturas — consume horas. ¿Y si un equipo de agentes (agent) especializados pudiera manejar todo esto en una noche?

Este flujo de trabajo establece una fábrica de contenido multi-agente dentro de Discord, donde diferentes agentes manejan investigación, escritura y material visual en canales dedicados.

## Descripción general de funciones

- **Agente de investigación** escanea cada mañana historias populares, contenido de competidores y redes sociales, encuentra las mejores oportunidades de contenido
- **Agente de escritura** toma las mejores ideas, escribe guiones completos, hilos de Twitter o borradores de boletines
- **Agente de miniaturas** genera miniaturas de AI o imágenes de portada para el contenido
- Cada agente trabaja en su propio canal de Discord, mantiene todo organizado y revisable
- Se ejecuta automáticamente según programación (por ejemplo, cada mañana a las 8 AM), te despiertas y ves el contenido completado

## Dolor

La creación de contenido tiene tres fases — investigación, escritura y diseño — la mayoría de los creadores completan estas tres fases manualmente. Incluso con herramientas de escritura de AI, todavía necesitas darles prompts uno por uno. Este sistema vincula agentes en una pipeline, la salida de un agente se convierte en la entrada del siguiente, completamente sin intervención humana.

## Habilidades requeridas

- Integración con Discord, configurar múltiples canales
- `sessions_spawn` / `sessions_send`, para orquestación multi-agente
- [x-research-v2](https://clawhub.ai) o herramientas similares, para investigación de redes sociales
- Generación de imágenes local (como Nano Banana) o API de generación de imágenes
- Skill [knowledge-base](https://clawhub.ai) (opcional, para investigación impulsada por RAG)

## Cómo configurar

1. Configurar un servidor de Discord (o dejar que OpenClaw te ayude a configurarlo — solo di "ayúdame a construir un Discord").

2. Crear canales para cada agente:
   - `#research` — Temas populares y oportunidades de contenido
   - `#scripts` — Borradores y esquemas escritos
   - `#thumbnails` — Imágenes y portadas generadas

3. Indicar a OpenClaw:

```text
I want you to build me a content factory inside of Discord.
Set up channels for different agents:

1. Research Agent (#research): Every morning at 8 AM, research top trending
   stories, competitor content, and what's performing well on social media
   in my niche. Post the top 5 content opportunities with sources.

2. Writing Agent (#scripts): Take the best idea from the research agent
   and write a full script/thread/newsletter draft. Post it in #scripts.

3. Thumbnail Agent (#thumbnails): Generate AI thumbnails or cover images
   for the content. Post them in #thumbnails.

Have all their work organized in different channels.
Run this pipeline automatically every morning.
```

4. Personalizar según tu plataforma:

Indicar a OpenClaw:

```text
I focus on X/Twitter threads, not YouTube. Change the writing agent
to produce tweet threads instead of video scripts.
```

## Ideas clave

- El núcleo está en **agentes encadenados** — la investigación impulsa la escritura, la escritura impulsa las miniaturas. No necesitas dar prompts gradualmente por separado.
- Los canales de Discord te permiten revisar fácilmente el trabajo de cada agente por separado, y proporcionar retroalimentación como "el guion es demasiado largo" o "enfócate más en noticias de AI".
- Puedes adaptar este patrón a cualquier formato de contenido: tweets, boletines, posts de LinkedIn, esquemas de podcasts, artículos de blog.
- Usar modelos locales para generación de imágenes (como ejecutar Nano Banana en Mac Studio) puede reducir costos y obtener más control.

## Fuentes de inspiración

Inspirado en [video de Alex Finn sobre casos de uso de OpenClaw que cambian la vida](https://www.youtube.com/watch?v=41_TNGDDnfQ).

## Enlaces relacionados

- [Documentación de sub-agentes de OpenClaw](https://github.com/openclaw/openclaw)
- [Guía de configuración de Discord Bot](https://discord.com/developers/docs)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/content-factory.md)
