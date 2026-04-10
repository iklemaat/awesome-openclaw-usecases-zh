# Resumen de Noticias Tecnológicas Multifuente

Agregar, calificar y enviar automáticamente noticias tecnológicas de 109+ fuentes, cubre RSS, Twitter/X, lanzamientos de GitHub y búsqueda web — todo gestionado vía lenguaje natural.

## Dolor

Quieres mantenerte al día con la dinámica de AI, open source y tecnología de vanguardia, necesitas revisar docenas de feeds RSS, cuentas de Twitter, repositorios de GitHub y sitios web de noticias diariamente. La curación manual consume mucho tiempo, y la mayoría de las herramientas existentes o carecen de filtrado de calidad o requieren configuración compleja.

## Qué puede hacer

Una pipeline de datos de cuatro capas que se ejecuta según programación:

1. **Feeds RSS** (46 fuentes) — OpenAI, Hacker News, MIT Technology Review, etc.
2. **KOLs de Twitter/X** (44 cuentas) — @karpathy, @sama, @VitalikButerin, etc.
3. **Lanzamientos de GitHub** (19 repositorios) — vLLM, LangChain, Ollama, Dify, etc.
4. **Búsqueda web** (4 búsquedas temáticas) — vía API de Brave Search.

Todos los artículos se combinan, eliminan duplicados por similitud de título, y se califican por calidad (priorizar fuente +3, multi-fuente +5, oportunidad +2, interacción +1). El resumen final se envía a Discord, correo electrónico o Telegram.

El marco es completamente personalizable — puedes agregar tus propios feeds RSS, cuentas de Twitter, repositorios de GitHub o consultas de búsqueda en 30 segundos.

## Prompt

**Instalar y configurar resumen diario:**
```text
Install tech-news-digest from ClawHub. Set up a daily tech digest at 9am to Discord #tech-news channel. Also send it to my email at myemail@example.com.
```

**Agregar fuentes personalizadas:**
```text
Add these to my tech digest sources:
- RSS: https://my-company-blog.com/feed
- Twitter: @myFavResearcher
- GitHub: my-org/my-framework
```

**Generar bajo demanda:**
```text
Generate a tech digest for the past 24 hours and send it here.
```

## Habilidades requeridas

- [tech-news-digest](https://clawhub.ai/skills/tech-news-digest) — Instalar vía `clawhub install tech-news-digest`
- [gog](https://clawhub.ai/skills/gog) (opcional) — Para enviar correos vía Gmail

## Variables de entorno (opcionales)

- `X_BEARER_TOKEN` — Bearer token de API de Twitter/X, para monitorear KOLs
- `BRAVE_API_KEY` — Clave de API de Brave Search, para capa de búsqueda web
- `GITHUB_TOKEN` — Token de GitHub, para mayor límite de tasa de API

## Enlaces relacionados

- [Repositorio de GitHub](https://github.com/draco-agent/tech-news-digest)
- [Página de ClawHub](https://clawhub.ai/skills/tech-news-digest)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/multi-source-tech-news-digest.md)
