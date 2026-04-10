# Búsqueda Semántica de Memoria

El sistema de memoria integrado de OpenClaw almacena todo como archivos Markdown — pero a medida que las memorias crecen durante semanas y meses, encontrar esa decisión del martes pasado se vuelve casi imposible. Sin búsqueda, solo puedes desplazarte por archivos.

Este caso de uso usa [memsearch](https://github.com/zilliztech/memsearch) para agregar **búsqueda semántica impulsada por vectores** sobre los archivos de memoria Markdown existentes de OpenClaw, permitiéndote encontrar instantáneamente cualquier memoria pasada por semántica en lugar de solo palabras clave.

## Qué puede hacer

- Indexar todos los archivos de memoria Markdown de OpenClaw en una base de datos de vectores (Milvus) con un comando
- Buscar por semántica: "¿Qué esquema de caché elegimos?" encuentra memorias relevantes incluso si la palabra "caché" no aparece en el documento
- Búsqueda híbrida (vector denso + búsqueda de texto completo BM25) con reordenamiento RRF (fusión de ranking recíproco) para mejores resultados
- Hash de contenido SHA-256 significa que archivos sin modificar nunca se re-embeben — cero llamadas a API desperdiciadas
- Vigilante de archivos re-indexa automáticamente cuando los archivos de memoria cambian, mantiene el índice siempre actualizado
- Soporta cualquier proveedor de embedding: OpenAI, Google, Voyage, Ollama, o ejecutar completamente local (sin clave de API)

## Dolor

La memoria de OpenClaw se almacena como archivos Markdown simples. Esto es bueno para portabilidad y legibilidad humana, pero no hay búsqueda. A medida que la memoria crece, o haces grep en archivos (solo palabras clave, pierde coincidencias semánticas), o cargas archivos completos en el contexto (gasta tokens en contenido irrelevante). Necesitas una forma de preguntar "¿Qué decisión tomé sobre X?" y obtener fragmentos precisamente relevantes, independientemente de la redacción.

## Habilidades requeridas

- Sin skills de OpenClaw necesarias — memsearch es un CLI/biblioteca independiente de Python
- Python 3.10+ con pip o uv

## Cómo configurar

1. Instalar memsearch:
```bash
pip install memsearch
```

2. Ejecutar asistente de configuración interactiva:
```bash
memsearch config init
```

3. Indexar tu directorio de memoria de OpenClaw:
```bash
memsearch index ~/path/to/your/memory/
```

4. Buscar en tus memorias por semántica:
```bash
memsearch search "what caching solution did we pick?"
```

5. Para sincronización en tiempo real, iniciar vigilante de archivos — indexa automáticamente en cada cambio de archivo:
```bash
memsearch watch ~/path/to/your/memory/
```

6. Para despliegue completamente local (sin clave de API), instalar proveedor de embedding local:
```bash
pip install "memsearch[local]"
memsearch config set embedding.provider local
memsearch index ~/path/to/your/memory/
```

## Ideas clave

- **Markdown siempre es la fuente de datos.** El índice vectorial es solo un caché derivado — puedes reconstruirlo en cualquier momento con `memsearch index`. Tus archivos de memoria nunca se modifican.
- **Desduplicación inteligente ahorra costos.** Cada chunk de texto se identifica por hash de contenido SHA-256. Re-ejecutar `index` solo embebe contenido nuevo o cambiado, así que puedes ejecutar libremente sin desperdiciar llamadas a API de embedding.
- **Búsqueda híbrida es mejor que solo vectores.** Combinar similitud semántica (vector denso) con coincidencia de palabras clave (BM25) vía Reciprocal Rank Fusion (fusión de ranking recíproco), captura tanto consultas basadas en semántica como coincidencias exactas.

## Enlaces relacionados

- [memsearch GitHub](https://github.com/zilliztech/memsearch) — La biblioteca que impulsa este caso de uso
- [Documentación de memsearch](https://zilliztech.github.io/memsearch/) — Referencia completa de CLI, API de Python y descripción de arquitectura
- [OpenClaw](https://github.com/openclaw/openclaw) — La arquitectura de memoria que inspiró memsearch
- [Milvus](https://milvus.io/) — Backend de base de datos de vectores

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/semantic-memory-search.md)
