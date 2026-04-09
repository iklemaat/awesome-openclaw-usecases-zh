# Guía de Contribución

Gracias por tu interés en este proyecto. Esta colección se compromete a proporcionar a los usuarios hispanohablantes casos de uso reales de OpenClaw **verificados** — ayudar a los lectores a encontrar "formas en que los agentes de IA pueden mejorar la vida", seguir los pasos para que funcionen.

Damos la bienvenida a las siguientes contribuciones:

- **Enviar nuevos casos de uso** (originales del ecosistema local o adaptación de casos internacionales)
- **Mejorar casos existentes** (complementar prácticas, corregir información desactualizada)
- **Mejorar traducción o expresión** (hacer que el contenido existente sea más preciso y natural)

---

## Criterios de Inclusión

Los casos de uso deben cumplir simultáneamente estas tres condiciones para ser incluidos:

| Condición | Descripción |
|------|------|
| **Ejecutado exitosamente** | Tiene evidencia de ejecución end-to-end — pasos de configuración, comandos, salida reproducible, no aceptamos "posts de ideas" |
| **Verificación multi-fuente** | Al menos dos fuentes independientes — "documentación oficial/repositorio" + "contenido de practicantes de la comunidad" |
| **Operable** | Tiene pasos concretos y prompts/configuraciones copiables, los lectores pueden reproducir siguiendo los pasos |

El contenido que no cumpla con los estándares no será mergeado, pero damos la bienvenida a discutir en Issues.

---

## Tipos de casos de uso

### 1. Casos originales del ecosistema local

Casos de uso diseñados específicamente para el ecosistema de herramientas local (Feishu, DingTalk, WeCom, Xiaohongshu, AKShare, etc.).

- Archivos en el directorio `usecases/`, nombre de archivo usa prefijo `cn-`, como `cn-feishu-ai-assistant.md`
- Agregar una fila en la tabla **Casos Adaptados Localmente** del README

### 2. Adaptación local de casos internacionales

Agregar soluciones alternativas locales sobre la base de casos internacionales existentes.

- **No modificar el contenido original**, agregar sección `## Adaptación para usuarios de China` al final del archivo
- Agregar línea de indicación debajo del título del archivo: `> Incluye adaptación local: XX / YY / ZZ`
- Agregar una fila en la tabla **Casos Adaptados Localmente** del README, nombre marcado como "（adaptado）"
- Mantener simultáneamente en la tabla de clasificación correspondiente abajo, nombre marcado como "（adaptación local）"

### 3. Mejorar casos existentes

Complementar prácticas, corregir información desactualizada, mejorar expresión. Modificar directamente el archivo correspondiente.

---

## Formato de archivos de casos de uso

Cada archivo de caso de uso se organiza según la siguiente estructura (ajustar flexiblemente según la situación real):

```markdown
# Nombre del caso de uso

Descripción breve del dolor y qué hace este caso (2-3 frases).

## Qué puede hacer

Listar puntos de funciones principales.

## Habilidades requeridas

Skills/plugins de OpenClaw que necesitan instalarse, indicar fuente.

## Cómo configurar

Explicar método de configuración paso a paso, incluye prompts y comandos copiables.

## Consejos prácticos

Errores encontrados, mejores prácticas, notas importantes.

## Enlaces relacionados

Documentación de referencia, repositorios de herramientas, artículos de la comunidad, etc.
```

**Puntos clave:**

- Prompts se recomiendan保留 el original en inglés, agregar explicación en español arriba
- Comentarios en bloques de código traducir al español, código y comandos保留 inglés
- Términos técnicos la primera vez que aparecen agregar paréntesis explicativo, como `webhook (gancho web)`
- Casos que involucran automatización de redes sociales deben incluir recordatorios de control de viento de la plataforma
- No hardcodear claves API, credenciales se pasan mediante variables de entorno

---

## Flujo de envío

1. Crear nueva rama desde rama `main`: `feat/nombre-caso`
2. Crear o modificar archivo de caso en `usecases/`
3. Actualizar README:
   - Agregar entrada en la tabla correspondiente
   - Actualizar badge de conteo de casos en la parte superior
4. Enviar PR a rama `main`, título usa descripción neutral

Convención de nombres de ramas: `feat/xxx` (nueva función), `fix/xxx` (corrección), `improve/xxx` (mejora)

---

## Contenido no aceptado

- Casos no probados realmente (generados por IA pero no ejecutados exitosamente no cuentan)
- Casos relacionados con criptomonedas
- Soluciones de protocolo WeChat personal (riesgo extremadamente alto de bloqueo de cuenta)
- API keys o credenciales hardcodeadas

---

## Convención de citaciones

- Priorizar citar documentación oficial y proyectos open source maduros
- Skills/plugins de la comunidad deben indicar fuente, permitir a los lectores juzgar riesgos por sí mismos
- No recomendar plugins de comunidad no verificados (repos con pocos stars no cuentan como verificados)
- Productos SaaS comerciales no se recomiendan en el cuerpo principal
