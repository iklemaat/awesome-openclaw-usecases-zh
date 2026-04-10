# Descubrimiento de Investigación de Papers de HuggingFace

> Incluye adaptación local: Espejo de HF / PaperWeekly / Push Feishu

Mantenerse al día con la vanguardia de la investigación en ML (aprendizaje automático) significa navegar diariamente la página de HuggingFace Papers, escanear docenas de títulos, abrir uno por uno para ver resúmenes, y luego cruzar manualmente con repositorios de GitHub. Necesitas una forma conversacional de descubrir, filtrar y leer profundamente papers populares sin salir de tu espacio de trabajo.

Este flujo de trabajo combina dos skills (habilidades), construye una pipeline completa de descubrimiento de investigación:

- Navegar papers populares del día en HuggingFace — ordenar por likes o fecha
- Buscar papers por palabra clave, localizar rápidamente trabajos relevantes de cualquier tema
- Obtener metadatos completos del paper: resumen, autores, repositorio de GitHub, likes de la comunidad, resumen generado por AI
- Leer discusiones y comentarios de la comunidad de cualquier paper
- Leer profundamente el código fuente LaTeX completo del paper vía ID de arXiv (usando skill arxiv-source)

> **Relación con el caso de uso de lectura de papers arXiv**: El [caso de uso de lectura de papers arXiv y escritura LaTeX](https://github.com/AlexAnys/awesome-openclaw-usecases-zh/pull/36) en este repositorio se enfoca en "lectura profunda y asistencia de escritura de papers conocidos" — análisis de texto completo, navegación por capítulos, escritura LaTeX después de dar ID de arXiv. Este caso de uso se enfoca en la **fase upstream: descubrimiento y filtrado** — populares diarios, búsqueda por palabra clave, navegación de metadatos, señales de comunidad. Son complementarios: primero usa este caso de uso para encontrar papers que vale la pena leer, luego usa el caso de uso arXiv para lectura profunda.

## Habilidades requeridas

- Skill [hf-papers](https://github.com/openclaw/skills/tree/main/skills/willamhou/hf-papers) (4 herramientas: `hf_daily_papers`, `hf_search_papers`, `hf_paper_detail`, `hf_paper_comments`)
- Skill [arxiv-source](https://github.com/openclaw/skills/tree/main/skills/willamhou/arxiv-source) (3 herramientas: `arxiv_fetch`, `arxiv_sections`, `arxiv_abstract`) — para lectura profunda de texto completo

No se necesita Docker ni autenticación — ambos skills usan APIs públicas, soportan caché local.

## Cómo configurar

1. Instalar dos skills:

```bash
clawhub install hf-papers
clawhub install arxiv-source
```

2. Enviar el siguiente prompt a OpenClaw:

```text
I want to stay on top of ML research. Here's my daily workflow:

1. Every morning, show me the top 10 trending papers on Hugging Face (sorted by upvotes)
   - For each paper, show: paper ID (the arXiv ID, e.g. "2505.12345"), title, upvotes, GitHub repo (if any), and 1-line AI summary

2. When I say "search [topic]":
   - Search HF Papers and show the most relevant results with their paper IDs
   - Highlight papers with linked GitHub repos or high upvote counts

3. When I pick a paper (by paper ID, e.g. "2505.12345"):
   - Show the full abstract, authors, and linked resources
   - Show community comments if any
   - Ask if I want a deep read

4. For deep reads:
   - Fetch the full paper via arxiv-source
   - Summarize key contributions, methodology, and results
   - Note any linked code repos I should check out

Keep a running list of papers I've reviewed today with one-line takeaways.
```

Descripción del prompt:
- Paso 1: Mostrar Top 10 populares de HF cada mañana, ordenar por likes, adjuntar ID de paper (ID de arXiv)/título/número de likes/repositorio de GitHub/resumen de AI
- Paso 2: Ingresar "search [tema]" para búsqueda por palabra clave, resaltar papers con repositorios de código o alto número de likes
- Paso 3: Después de seleccionar paper mostrar resumen completo, autores, recursos relacionados, comentarios de la comunidad
- Paso 4: Modo lectura profunda — obtener texto completo vía arxiv-source, resumir contribuciones clave/método/conclusiones
- Última línea: Mantener lista de papers revisados hoy, adjuntar resumen en una línea

3. Probar: "What's trending on HuggingFace Papers today?"

## Consejos prácticos

- **Estrategia de filtrado**: Primero ver número de likes y comentarios para filtrar rápidamente, luego ver resumen para decidir si leer profundamente. El número de likes de la comunidad es una señal efectiva para juzgar la popularidad del paper
- **Combinación de palabras clave**: Intentar diferentes granularidades de palabras clave al buscar, por ejemplo "vision transformer" vs "ViT efficient inference", reducir el alcance puede reducir ruido
- **Asociación de repositorios de GitHub**: Papers con repositorios de GitHub asociados generalmente significan que el código reproducible está abierto, priorizar estos papers
- **Comentarios de la comunidad**: La herramienta hf_paper_comments puede obtener discusiones de la comunidad, a veces los comentarios tienen explicaciones suplementarias del autor sobre limitaciones del método
- **Combinar con tareas programadas**: Se puede usar la función cron job (tarea programada) de OpenClaw, enviar automáticamente resúmenes de papers populares cada mañana

## Adaptación para usuarios de China

### Problemas de acceso a HuggingFace

HuggingFace tiene acceso restringido en China continental. Los siguientes son esquemas alternativos disponibles:

| Esquema | Descripción |
|------|------|
| **hf-mirror.com** | Espejo de HF mantenido por la comunidad, configurar `HF_ENDPOINT=https://hf-mirror.com` funciona. Este espejo ya cubre la API de Papers (`/api/daily_papers`, `/api/papers/search`, etc.), el skill hf-papers puede funcionar normalmente vía este espejo |
| **huggingface.ac.cn** | Espejo oficial de cooperación de HF en China, incluye página seleccionada de papers de AI |
| Acceso por proxy | Configurar proxy de red para acceder al sitio original por ti mismo |

> **Consejo**: Si el skill hf-papers no soporta la variable de entorno `HF_ENDPOINT`, necesitas modificar manualmente la URL base de la API en el código fuente del skill, reemplazar `huggingface.co` por `hf-mirror.com`.

### Comunidades de papers de AI nacionales

Además de HuggingFace Papers, también hay canales de descubrimiento de papers de AI de alta calidad nacionales:

| Plataforma | Características | Enlace |
|------|------|------|
| **PaperWeekly** | Recomendación e interpretación de papers académicos, tiene columna de Zhihu y cuenta oficial de WeChat | [Zhihu](https://zhuanlan.zhihu.com/paperweekly) |
| **Máquina de Corazones (Synced)** | Reportajes profundos en área de AI, incluye interpretación de papers | [Sitio oficial](https://www.jiqizhixin.com/) |
| **Comunidad de BAAI** | Comunidad del Instituto de Investigación de Inteligencia Artificial de Beijing, incluye boletín de papers | [Sitio oficial](https://hub.baai.ac.cn/) |
| **huggingface.ac.cn** | Espejo de HF en China, página seleccionada de papers diarios | [Página de papers](https://huggingface.ac.cn/papers) |

Estas plataformas pueden servir como fuentes de información complementarias para HuggingFace Papers, especialmente proporcionando canales alternativos de descubrimiento cuando la red está restringida.

### Adaptación de canal de push

| Esquema original | Reemplazo nacional | Descripción |
|---------|---------|------|
| Chat de terminal | **Bot de Feishu** | Soporta mensajes de tarjeta de texto enriquecido, adecuado para enviar resúmenes de papers |
| Chat de terminal | **Bot de grupo de DingTalk** | Forma Webhook, configuración más simple |
| Chat de terminal | **Aplicación de WeCom** | Primera opción para usuarios empresariales |

### Adaptación de prompt

Si estás más acostumbrado a la interacción en chino, puedes usar el siguiente prompt:

```text
I want to track ML research daily. Here's my workflow:

1. Every morning at 9 AM, fetch the top 10 trending papers from Hugging Face Papers (sorted by upvotes)
   - Show: paper ID (arXiv ID), title, upvotes, GitHub repo link (if available), 1-line summary
   - Format as a numbered list for easy reference

2. After showing the list, wait for my instructions:
   - If I say a number (e.g. "3"), show the full details for that paper
   - If I say "search [keyword]", search for papers on that topic

3. For paper details, always include:
   - Full abstract
   - Author list
   - Linked GitHub repos
   - Community comments (if any)

4. When I say "deep read":
   - Use arxiv-source to fetch the full paper text
   - Summarize: key contributions, methodology, main results, limitations

5. At end of each session, compile a digest of all papers I reviewed today with one-line takeaways.

Respond in Chinese for summaries and explanations, but keep paper titles and technical terms in English.
```

Descripción del prompt:
- Paso 1: Obtener Top 10 populares de HF a las 9 AM cada día, adjuntar ID de paper (ID de arXiv), mostrar en formato de lista numerada
- Paso 2: Ingresar número para ver detalles, ingresar "search [palabra clave]" para buscar
- Paso 3: Detalles de paper incluyen resumen, autores, enlace de GitHub, comentarios de la comunidad
- Paso 4: Ingresar "deep read" para iniciar modo de lectura profunda
- Paso 5: Resumir papers revisados al final de cada sesión
- Última línea: Responder resúmenes y explicaciones en chino, mantener títulos de papers y términos técnicos en inglés

## Enlaces relacionados

- [Skill de hf-papers](https://github.com/openclaw/skills/tree/main/skills/willamhou/hf-papers) — Navegación y búsqueda de papers de HuggingFace
- [Skill de arxiv-source](https://github.com/openclaw/skills/tree/main/skills/willamhou/arxiv-source) — Obtención de texto completo de papers arXiv
- [Papers Diarios de HuggingFace](https://huggingface.co/papers) — Selección diaria de papers de AI
- [Documentación de API de HF Papers (no oficial)](https://github.com/0x0is1/hf-papers-api-docs) — Referencia de endpoints de API de HuggingFace Papers
- [Página de papers de huggingface.ac.cn](https://huggingface.ac.cn/papers) — Selección de papers del espejo de HF en China

---

**Enlace original**: [Versión en inglés](https://github.com/hesamsheikh/awesome-openclaw-usecases/blob/main/usecases/hf-papers-research-discovery.md)
