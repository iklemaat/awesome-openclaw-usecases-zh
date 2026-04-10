# Base de Conocimientos Personal (RAG)

Pasas todo el día leyendo artículos, tweets y viendo videos, pero nunca encuentras esa cosa que viste la semana pasada. Los marcadores se acumulan y eventualmente se vuelven inútiles.

Este flujo de trabajo construye una base de conocimientos buscable a partir de todo el contenido que guardas:

- Insertar cualquier URL en Telegram o Slack, el sistema automáticamente ingiere el contenido (artículos, tweets, transcripciones de YouTube, PDFs)
- Búsqueda semántica en todo tu contenido guardado: buscar "¿qué guardé sobre memoria de agentes?" devuelve resultados clasificados con fuentes
- Se puede联动 con otros flujos de trabajo — por ejemplo, la pipeline de ideas de video consulta la base de conocimientos para contenido guardado relevante al construir tarjetas de investigación

## Habilidades requeridas

- Skill [knowledge-base](https://clawhub.ai) (o construir sistema RAG (generación aumentada por recuperación) con embedding (vectores) personalizado)
- `web_fetch` (integrado)
- Tema de Telegram o canal de Slack para ingestión de contenido

## Cómo configurar

1. Instalar skill knowledge-base desde ClawdHub.
2. Crear un tema de Telegram llamado "knowledge-base" (o usar canal de Slack).
3. Enviar el siguiente prompt a OpenClaw:
```text
When I drop a URL in the "knowledge-base" topic:
1. Fetch the content (article, tweet, YouTube transcript, PDF)
2. Ingest it into the knowledge base with metadata (title, URL, date, type)
3. Reply with confirmation: what was ingested and chunk count

When I ask a question in this topic:
1. Search the knowledge base semantically
2. Return top results with sources and relevant excerpts
3. If no good matches, tell me

Also: when other workflows need research (e.g., video ideas, meeting prep), automatically query the knowledge base for relevant saved content.
```

4. Probar: dejar caer algunas URLs, luego hacer preguntas como "¿Qué tengo sobre memoria de LLM?"

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/knowledge-base-rag.md)
