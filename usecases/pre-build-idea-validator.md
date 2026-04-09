# Validador de Ideas Pre-Desarrollo

> Incluye adaptación local: Índice Baidu / Índice WeChat / V2EX / Minority

Antes de que OpenClaw comience a escribir código, automáticamente verifica si tu idea ya existe — escanea 5 fuentes de datos principales (GitHub, Hacker News, npm, PyPI y Product Hunt), decide el siguiente paso según el nivel de competencia.

## Qué puede hacer

- Antes de escribir cualquier código, automáticamente escanea 5 fuentes de datos reales (GitHub, Hacker News, npm, PyPI, Product Hunt)
- Devuelve puntuación de competencia `reality_signal` (0-100), muestra intuitivamente el nivel de congestión del área
- Muestra competidores principales con su conteo de Stars y descripción
- Cuando el área está saturada, automáticamente sugiere direcciones de diferenciación
- Como "punto de control" antes del desarrollo: puntuación alta = detenerse a discutir, puntuación baja = comenzar directamente

## Dolor

Le dices al agente "ayúdame a hacer una herramienta de revisión de código con IA", felizmente escribe código por 6 horas. Sin embargo, ya hay 143,000+ repositorios relacionados en GitHub — el número uno tiene 53,000 Stars. El agente nunca verifica, porque no se lo pediste, no sabe que debe verificar. Después de invertir mucho tiempo descubres que los competidores ya son maduros. Este patrón se repite en cada nuevo proyecto.

## Habilidades requeridas

- [idea-reality-mcp](https://github.com/mnemox-ai/idea-reality-mcp) — Servidor MCP que escanea fuentes de datos reales y devuelve puntuación de competencia

## Cómo configurar

1. Instalar idea-reality-mcp:

```bash
uvx idea-reality-mcp
```

2. Agregar servidor MCP en configuración de OpenClaw:

```json
{
  "mcpServers": {
    "idea-reality": {
      "command": "uvx",
      "args": ["idea-reality-mcp"]
    }
  }
}
```

3. Agregar las siguientes instrucciones a tu configuración de agente OpenClaw:

```text
Before starting any new project, feature, or tool, always run idea_check first.

Rules:
- If reality_signal > 70: STOP. Report the top 3 competitors with star counts.
  Ask me if I want to proceed, pivot, or abandon.
- If reality_signal 30-70: Show me the results and pivot_hints.
  Suggest a niche angle that existing projects don't cover.
- If reality_signal < 30: Proceed to build.
  Mention that the space is open.
- Always show the reality_signal score and top competitors before writing any code.
```

## Casos reales

**Tú:** "Ayúdame a hacer una herramienta CLI de revisión de código con AI"

OpenClaw ejecuta `idea_check("AI code review tool")` y responde:

> **reality_signal: 90/100 (muy alto)**
>
> Competidores principales:
> 1. Gitea — 53,940 stars
> 2. reviewdog — 9,104 stars
> 3. Danger (Ruby) — 5,649 stars
>
> Hay 143,000+ repositorios relacionados en esta área, productos principales maduros y bien financiados.
>
> **Sugerencias de diferenciación:**
> - Enfocarse en lenguaje específico (revisión de código AI para Rust/Go)
> - Enfocarse en framework específico (revisión de componentes React/Vue)
> - Enfocarse en industria específica (revisión de cumplimiento de código para finanzas/salud)
>
> ¿Quieres continuar con versión diferenciada, o cambiar de dirección?

**Tú:** "¿Y hacer un servidor MCP de validación de ideas pre-desarrollo?"

> **reality_signal: 8/100 (muy bajo)**
>
> No hay competidores directos en PyPI y npm. Hay algunos repositorios relacionados en GitHub, pero ninguno resuelve exactamente el mismo problema.
>
> Esta área está completamente en blanco. Comienza el desarrollo.

## Juego avanzado

- **Modo profundo**: Usar `depth="deep"` para escanear las 5 fuentes de datos simultáneamente (GitHub + HN + npm + PyPI + Product Hunt), adecuado para decisiones de proyectos mayores.
- **Validación por lotes**: Antes de hackathon, dar a OpenClaw una lista de 10 ideas, dejar que clasifique por `reality_signal` — puntuación más baja = oportunidad más original.
- **Primero prueba versión Web**: Prueba gratis en [mnemox.ai/check](https://mnemox.ai/check), ve si este flujo de trabajo se adapta a tus necesidades.

## Ideas clave

- Esto previene el error más costoso en desarrollo: **resolver un problema que ya fue resuelto**.
- `reality_signal` se basa en datos reales (cantidad de repositorios, distribución de Stars, volumen de discusiones en HN), no en suposiciones de LLM.
- Puntuación alta no significa "no lo hagas" — sino "diferenciate o no pierdas tiempo".
- Puntuación baja significa verdadero océano azul. Esta es la dirección donde desarrolladores individuales tienen mayor probabilidad de éxito.

## Adaptación para usuarios de China

### idea-reality-mcp ya soporta chino

La versión v0.3.0 agregó pipeline de extracción de palabras clave de tres etapas, incluye 150+ mapeos de términos chino-inglés (cubre 15+ áreas), puedes describir tu idea directamente en chino. Por ejemplo, ingresar "app de reserva de veterinaria para mascotas" se mapea automáticamente a "pet appointment veterinary booking app" para buscar.

### Descripción de fuentes de datos

Las 5 fuentes de datos que escanea idea-reality-mcp (GitHub, Hacker News, npm, PyPI, Product Hunt) son todas plataformas internacionales. Para proyectos orientados al mercado nacional, se recomienda complementar con las siguientes fuentes de datos nacionales para validación cruzada:

| Fuente de datos internacional | Plataforma nacional correspondiente | Uso |
|-----------|-------------|------|
| Product Hunt | Sección [Creatividad] de V2EX, Minority | Descubrir nuevos productos y competidores nacionales |
| Hacker News | V2EX, Juejin, CSDN | Calor de discusiones en comunidad técnica |
| GitHub | Gitee | Búsqueda de proyectos open source nacionales |
| npm / PyPI | npm (universal) / Espejo Gitee | Gestión de paquetes y búsqueda de dependencias |

### Validación补充 de datos de mercado nacional

Después de que idea-reality-mcp da puntuación de competencia internacional, se recomienda verificar adicionalmente la demanda del mercado a través de plataformas de datos nacionales:

- **Índice Baidu** (index.baidu.com): Ver tendencias de búsqueda de palabras clave, juzgar si usuarios buscan activamente el problema que quieres resolver
- **Índice WeChat**: Ver calor de palabras clave en ecosistema WeChat dentro de mini-programa de WeChat, adecuado para validación de productos C-end
- **巨量算数** (trendinsight.oceanengine.com): Datos de ecosistema Douyin/toutiao, entender tendencias en área de videos cortos y contenido
- **Índice Alibaba / Consejero de negocios**: Validación de demanda en área de e-commerce, ver tendencias de búsqueda de productos y nivel de competencia
- **36Kr, IT Juzi**: Ver si ya hay proyectos financiados en esta área, evaluar panorama competitivo

### Prompt sugerido para complementar

Sobre la base del prompt original, se pueden agregar las siguientes reglas para cubrir el mercado nacional:

```text
After running idea_check, also help me research the Chinese market:
- Search for similar products on V2EX, 少数派，and 36 氪
- Check Baidu Index trends for related keywords
- Look for competing projects on Gitee
- Summarize whether the idea has more or less competition in China vs. internationally
```

### Escenarios aplicables

El valor de este caso de uso es especialmente prominente para desarrolladores y emprendedores nacionales:

- **Desarrolladores independientes**: Verificar rápidamente la competencia global de ideas antes de lanzar en Product Hunt o comunidades nacionales
- **Equipos de salida al mar**: Verificar espacio competitivo en mercado internacional, encontrar ángulos de diferenciación
- **Participantes de hackathon**: Filtrar temas más originales por lotes
- **Gerentes de producto**: Hablar con datos antes de establecer proyecto, evitar reinventar la rueda

## Enlaces relacionados

- [idea-reality-mcp GitHub](https://github.com/mnemox-ai/idea-reality-mcp)
- [Experiencia en línea Web](https://mnemox.ai/check) (se puede probar sin instalar)
- [PyPI](https://pypi.org/project/idea-reality-mcp/)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/pre-build-idea-validator.md)
