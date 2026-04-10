# Pipeline de Contenido de YouTube

Como creador de YouTube que publica diariamente, encontrar ideas de contenido frescas y oportunas en la web y X/Twitter consume mucho tiempo. Rastrear contenido ya cubierto evita repetir temas y te ayuda a mantener la delantera de las tendencias.

Este flujo de trabajo automatiza toda la pipeline de búsqueda e investigación de contenido:

- Tarea programada cada hora (cron job) escanea noticias repentinas de AI (web + X/Twitter), y envía ideas de video a Telegram
- Mantiene un directorio de videos de 90 días, con análisis de vistas y temas, para evitar repetir temas
- Almacena todas las ideas en una base de datos SQLite, y usa embedding vectorial para desduplicación semántica (así nunca recibirás recomendaciones de ideas duplicadas)
- Cuando compartes un enlace en Slack, OpenClaw investiga el tema, busca publicaciones relacionadas en X, consulta tu base de conocimientos, y crea una tarjeta de tarea en Asana con un esquema completo

## Habilidades requeridas

- `web_search` (integrado)
- Skill [x-research-v2](https://clawhub.ai) o skill personalizado de búsqueda de X/Twitter
- Skill [knowledge-base](https://clawhub.ai), para RAG (generación aumentada por recuperación)
- Integración con Asana (o Todoist)
- CLI `gog`, para datos de YouTube Analytics
- Tema de Telegram, para recibir envíos de ideas

## Cómo configurar

1. Configurar un tema de ideas de video en Telegram, y configurar en OpenClaw.
2. Instalar el skill knowledge-base y el skill x-research.
3. Crear la base de datos SQLite para seguimiento de ideas:

```sql
CREATE TABLE pitches (
  id INTEGER PRIMARY KEY,
  timestamp TEXT,
  topic TEXT,
  embedding BLOB,
  sources TEXT
);
```

4. Indicar a OpenClaw:

```text
Run an hourly cron job to:
1. Search web and X/Twitter for breaking AI news
2. Check against my 90-day YouTube catalog (fetch from YouTube Analytics via gog)
3. Check semantic similarity against all past pitches in the database
4. If novel, pitch the idea to my Telegram "video ideas" topic with sources

Also: when I share a link in Slack #ai_trends, automatically:
1. Research the topic
2. Search X for related posts
3. Query my knowledge base
4. Create an Asana card in Video Pipeline with a full outline
```

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/youtube-content-pipeline.md)
