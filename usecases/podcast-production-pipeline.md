# Línea de Producción de Podcasts

> Incluye adaptación local: Lanzamiento en Xiaoyuzhou / Ximalaya / Bilibili / Promoción en Xiaohongshu

Tienes un montón de temas de podcasts, incluso has hecho una lista de pendientes para grabar. Pero entre la investigación de invitados, escribir esquemas, redactar introducciones, organizar notas del programa, promoción en redes sociales — la sobrecarga de producción consume toda tu pasión creativa. ¿Y si solo necesitaras dar un tema, y recibieras un paquete de producción completo listo para publicar?

Este caso de uso encadena múltiples agentes, maneja el flujo completo de producción de podcasts desde el tema hasta el lanzamiento.

## Dolor

Los presentadores de podcasts independientes y pequeños equipos gastan mucho más tiempo en producción que en la grabación en sí. La investigación de invitados puede tomar horas, las notas del programa siempre se posponen, y la promoción en redes sociales es el primer paso que se omite. La parte realmente creativa — la conversación en sí — puede representar solo el 30% del trabajo total. Este agente te ayuda a manejar el 70% restante.

## Qué puede hacer

- **Investigación del programa** — Dado un tema o nombre de invitado, automáticamente organiza antecedentes, puntos de conversación y preguntas sugeridas
- **Esquema y guion** — Genera esquema estructurado del programa, incluye guion de apertura, transiciones de segmentos y cierre
- **Notas del programa** — Después de grabar, procesa la transcripción en notas del programa con timestamps, adjunta todos los enlaces mencionados
- **Paquete de material para redes sociales** — Genera posts promocionales con aspectos destacados y citas memorables del programa para X, LinkedIn e Instagram
- **Descripción del programa** — Escribe descripciones de programas optimizadas para SEO para Spotify, Apple Podcasts y YouTube

## Habilidades requeridas

- Habilidades de búsqueda web / investigación (para investigación de invitados e investigación profunda de temas)
- Acceso a sistema de archivos (para leer transcripciones y escribir archivos de salida)
- Integración con Slack, Discord o Telegram (para entrega de productos terminados)
- Opcional: `sessions_spawn` (para ejecutar agentes de investigación y escritura en paralelo)
- Opcional: Skill de feed RSS (para monitorear podcasts de competencia)

## Cómo configurar

1. Antes de grabar — generar materiales de investigación y esquema del programa:

```text
I'm recording a podcast episode about [TOPIC]. My guest is [NAME].

Please:
1. Research the guest — their background, recent work, hot takes, and
   anything controversial or interesting they've said publicly.
2. Research the topic — key trends, recent news, common misconceptions,
   and what the audience likely already knows vs. what would surprise them.
3. Generate an episode outline:
   - Cold open hook (1-2 sentences to grab attention)
   - Intro script (30 seconds, casual tone)
   - 5-7 interview questions, ordered from easy/rapport-building to deep/provocative
   - 2-3 "back pocket" questions in case the conversation stalls
   - Closing segment with call-to-action

Save everything to ~/podcast/episodes/[episode-number]/prep/
```

2. Después de grabar — generar notas del programa y material promocional:

```text
Here's the transcript for Episode [NUMBER]: [paste or point to file]

Please:
1. Write timestamped show notes — every major topic shift gets a timestamp
   and one-line summary. Include links to anything mentioned (tools, books,
   articles, people).
2. Write an episode description (max 200 words) optimized for podcast
   search. Include 3-5 relevant keywords naturally.
3. Create social media posts:
   - X/Twitter: 3 tweets — one pull quote, one key insight, one question
     to spark discussion. Each under 280 chars.
   - LinkedIn: 1 post, professional tone, 100-150 words.
   - Instagram caption: 1 post with emoji, casual tone, include relevant hashtags.
4. Extract a "highlights" list — the 3 most interesting/surprising moments
   with timestamps.

Save everything to ~/podcast/episodes/[episode-number]/publish/
```

3. Opcional — Monitoreo de podcasts de competencia:

```text
Monitor these podcast RSS feeds daily:
- [feed URL 1]
- [feed URL 2]

When a new episode drops that covers a topic relevant to my podcast,
send me a Telegram message with:
- Episode title and link
- One-sentence summary
- Whether this is something I should respond to or cover from my angle
```

## Ideas clave

- **Investigación pre-grabación** es donde está el mayor valor. Entrar al estudio con investigación profunda de invitados hace que la calidad de la conversación mejore cualitativamente — esto no se puede compensar en post-producción.
- Las **notas del programa con timestamps** son un truco poderoso para mejorar la retención de oyentes. La mayoría de los presentadores de podcasts saltan este paso porque es tedioso, pero el agente lo hace sin esfuerzo.
- El **paquete de material para redes sociales** es lo que ahorra más tiempo *continuo*. Cada episodio necesita promoción, la estructura es exactamente la misma — este es el escenario perfecto para automatización.
- Funciona aún mejor combinado con la [Fábrica de Contenido Multi-Agente](content-factory.md), puedes convertir contenido de podcasts en artículos de blog, boletines o clips de video.

## Adaptación para usuarios de China

El ecosistema de podcasts nacional tiene diferencias significativas con el extranjero, las siguientes son sugerencias de adaptación para creadores de podcasts chinos.

### Esquemas alternativos de plataformas

| Plataforma extranjera | Reemplazo nacional | Descripción |
|---------|---------|------|
| Spotify for Podcasters | Xiaoyuzhou (小宇宙) | Plataforma de podcasts pura más activa en China, soporta suscripción RSS y hosting, tiene backend de creador completo |
| Apple Podcasts | Ximalaya (喜马拉雅) | Plataforma de audio con mayor volumen de usuarios, necesita subir por separado (no soporta importación RSS pura) |
| YouTube (versión en video de podcast) | Podcast de NetEase Cloud Music / Bilibili | NetEase soporta importación RSS; Bilibili es adecuado para podcasts en video |
| Anchor | Lizhi FM / Qingting FM | Puede usarse como canal de distribución complementario |
| Slack/Discord | Feishu / DingTalk / WeChat | Para recibir paquetes de producción entregados por el agente |

### Estrategia de distribución

La distribución de podcasts nacionales se divide en dos categorías:

- **Distribución RSS** (Xiaoyuzhou, Apple Podcasts, NetEase Cloud Music, etc., clientes de tipo universal): Generar un enlace RSS, enviar a cada plataforma se sincroniza automáticamente
- **Subida manual** (Ximalaya, Lizhi FM, etc.): Necesita subir archivos de audio e información del programa por separado, puedes dejar que el agente prepare los metadatos que cada plataforma necesita (título, descripción, etiquetas), reducir trabajo repetitivo

Se recomienda usar plataformas de hosting como Firstory para generar RSS automáticamente, luego complementar manualmente las plataformas que necesitan subida individual.

### Adaptación de herramientas de producción

- **Transcripción**: Después de grabar se puede usar [Whisper](https://github.com/openai/whisper) para transcripción local, también se pueden usar servicios nacionales como iFlytek Hearing, Feishu Minutes, etc., la precisión de reconocimiento en chino es mayor
- **Edición**: Xiaoyuzhou proporciona la herramienta de edición en línea "Xiaoyuzhou Studio", integrada profundamente con el backend del presentador; también se pueden usar herramientas profesionales como Adobe Audition, DaVinci, etc.
- **Portadas y material visual**: Cooperar con herramientas de generación de imágenes de AI para crear portadas de cada episodio, mantener consistencia de marca

### Adaptación de promoción en redes sociales

Se recomienda reemplazar el prompt de promoción posterior a la grabación con plataformas nacionales:

```text
Here's the transcript for Episode [NUMBER]: [paste or point to file]

Please:
1. Write timestamped show notes (same as original).
2. Write an episode description (max 200 words) with 3-5 keywords,
   optimized for 小宇宙 and 喜马拉雅 search.
3. Create social media posts:
   - 小红书：1 post, conversational tone, include 5-8 hashtags,
     highlight the most relatable listener moment.
   - 微信公众号：1 article draft, 800-1200 words, deeper recap
     of episode themes with key quotes.
   - 微博：2 posts — one pull quote with guest photo prompt,
     one question to spark discussion. Each under 140 chars.
4. Extract a "highlights" list — the 3 most interesting moments
   with timestamps, formatted for short video clips (Douyin/Video Account).

Save everything to ~/podcast/episodes/[episode-number]/publish/
```

### Adaptación de monitoreo de competencia

Reemplazar los objetivos de monitoreo de RSS con podcasts nacionales que te interesan. La mayoría de los podcasts en Xiaoyuzhou proporcionan enlaces RSS, se pueden encontrar en la página del programa.

### Recordatorios especiales para creadores nacionales

- **Ciclo de producción de podcasts en chino es largo**: Según estadísticas de la comunidad de podcasts CPA, en 2024 los creadores de podcasts en chino promediaron 12.9 horas de trabajo neto por episodio,其中 la edición promedió 4.5 horas. Esto significa que la automatización desde investigación, notas del programa hasta promoción puede reducir significativamente la carga de trabajo no central
- **La mayoría son creadores a tiempo parcial**: Casi el 80% de los profesionales de podcasts son状态 a tiempo parcial, el tiempo de producción es fragmentado. Usar la automatización de pipelines del agente para trabajar, puede concentrar el tiempo limitado en el contenido en sí
- **Tendencia de podcasts en video**: En 2026 los podcasts en video están pasando de "forma complementaria" a "forma principal". Considerar agregar el paso automatizado de cortes de video corto (Douyin/Video Account) en la pipeline

## Enlaces relacionados

- [Especificación de Feed RSS de Podcast (Apple)](https://podcasters.apple.com/support/823-podcast-requirements)
- [Spotify for Podcasters](https://podcasters.spotify.com/)
- [Whisper (OpenAI)](https://github.com/openai/whisper) — Generación de transcripción local
- [Ingreso de presentador de Xiaoyuzhou](https://podcaster.xiaoyuzhoufm.com/)
- [Centro de creación de Ximalaya](https://zhubo.ximalaya.com/)

---

**Enlace original**: [Versión en inglés](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/podcast-production-pipeline.md)
