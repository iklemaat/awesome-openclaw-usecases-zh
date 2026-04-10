# Lectura de Papers arXiv y Escritura de Papers LaTeX

> Incluye adaptación local: Configuración LaTeX en chino / Espejo de Docker en China / Plantillas de tesis universitarias / Comparación de plataformas en línea
>
> 📝 Este documento combina los casos de uso arxiv-paper-reader y latex-paper-writing upstream, porque comparten la cadena de herramientas Prismer y el contenido de adaptación se superpone altamente.

Leer papers de arXiv significa descargar PDFs, cambiar frecuentemente entre múltiples papers perdiendo contexto, interpretar con dificultad fórmulas LaTeX densas. Escribir papers significa instalar TeX Live que ocupa varios GB, depurar errores de compilación, cambiar entre editor y vista previa de PDF. ¿Y si tu agente pudiera ayudarte a completar todo el flujo de lectura, análisis, escritura y compilación de papers en una conversación?

Este caso de uso integra dos flujos de trabajo académicos en un solo agente:

- **Lectura de papers**: Obtener paper vía ID de arXiv, aplanar automáticamente el código fuente LaTeX a texto legible, navegar estructura de capítulos, comparar resúmenes por lotes, dejar que el agente haga resumen y análisis
- **Escritura de papers**: Colaborar para escribir paper LaTeX en conversación, compilar instantáneamente a vista previa de PDF, soporta pdflatex/xelatex/lualatex, no necesita instalar entorno TeX local

## Dolor

- **Baja eficiencia de lectura**: Los papers de arXiv se publican en formato PDF, difícil localizar rápidamente capítulos clave; contexto se pierde frecuentemente al rastrear múltiples papers relacionados simultáneamente
- **Configuración de entorno tediosa**: Instalar TeX Live completo necesita varios GB de espacio, dependencias de diferentes plantillas de papers varían, mensajes de error de compilación son crípticos
- **Cambio frecuente de herramientas**: Leer usa una herramienta, escribir usa un editor, compilar usa línea de comandos, vista previa usa lector de PDF — flujo interrumpido

## Qué puede hacer

| Función | Descripción |
|------|------|
| **Obtención de papers** | Obtener paper vía ID de arXiv, descargar automáticamente y aplanar código fuente LaTeX (eliminar anidación `\input`) a texto limpio legible |
| **Navegación de capítulos** | Listar primero la estructura de capítulos del paper, decidir qué partes leer en profundidad, en lugar de leer todo el texto |
| **Escaneo rápido de resúmenes** | Obtener resúmenes de múltiples papers por lotes, generar tabla comparativa, ordenar por relevancia de dirección de investigación |
| **Análisis inteligente** | Dejar que el agente resuma contribuciones clave, analice metodología, evalúe resultados experimentales, compare diferentes papers |
| **Caché local** | Papers leídos se cachean localmente, acceso instantáneo la próxima vez |
| **Escritura LaTeX** | Describir contenido de párrafos en lenguaje natural, el agente genera el código fuente LaTeX correspondiente |
| **Compilación instantánea** | Soporta compilación pdflatex, xelatex, lualatex, no necesita instalar entorno TeX local |
| **Vista previa de PDF** | Vista previa de PDF directamente en la conversación después de compilar, no necesita cambiar de aplicación |
| **Soporte de plantillas** | Plantillas de inicio integradas article, IEEE, beamer, artículos en chino, etc. |
| **Referencias** | Soporta BibTeX/BibLaTeX, pegar contenido `.bib` se integra automáticamente |

## Habilidades requeridas

### Lectura de papers

- Skill [arxiv-reader](https://github.com/Prismer-AI/Prismer/tree/main/skills/arxiv-reader) (3 herramientas: `arxiv_fetch`, `arxiv_sections`, `arxiv_abstract`)

El skill arxiv-reader no necesita Docker ni Python, se ejecuta usando módulos integrados de Node.js. Descarga directamente el código fuente LaTeX desde arXiv, descomprime y aplana automáticamente referencias `\input`.

### Escritura de papers

- Skill [latex-compiler](https://github.com/Prismer-AI/Prismer/tree/main/skills/latex-compiler) (4 herramientas: `latex_compile`, `latex_preview`, `latex_templates`, `latex_get_template`)
- Contenedor de espacio de trabajo [Prismer](https://github.com/Prismer-AI/Prismer) (ejecuta servicio LaTeX en puerto 8080, incluye entorno completo de TeX Live con el contenedor)

## Cómo configurar

### 1. Instalar skill de lectura de papers

Instalar skill `arxiv-reader` desde el repositorio de Prismer — copiar directorio `skills/arxiv-reader/` a tu carpeta de skills de OpenClaw:

```bash
git clone https://github.com/Prismer-AI/Prismer.git
cp -r Prismer/skills/arxiv-reader/ ~/openclaw/skills/
```

Después de instalar el skill se puede usar directamente, sin configuración adicional.

### 2. Desplegar entorno de escritura de papers

Clonar y desplegar Prismer vía Docker (servicio LaTeX y TeX Live completo se inician automáticamente con el contenedor):

```bash
git clone https://github.com/Prismer-AI/Prismer.git && cd Prismer
docker compose -f docker/docker-compose.dev.yml up
```

El skill `latex-compiler` es un skill integrado, no necesita instalación separada.

### 3. Configurar prompt de lectura de papers

Enviar el siguiente prompt a OpenClaw para configurar flujo de trabajo de lectura de papers:

> Se recomienda保留 el siguiente prompt en inglés para mejor efecto. Las descripciones en chino te ayudan a entender el propósito de cada paso.

```text
I'm researching [topic]. Here's my workflow:

1. When I give you an arXiv ID (like 2301.00001):
   - First fetch the abstract so I can decide if it's relevant
   - If I say "read it", fetch the full paper (remove appendix by default)
   - Summarize the key contributions, methodology, and results

2. When I give you multiple IDs:
   - Fetch all abstracts and give me a comparison table
   - Rank them by relevance to my research topic

3. When I ask about a specific section:
   - List the paper's sections first
   - Then fetch and explain the relevant section in detail

Keep a running list of papers I've read and their key takeaways.
```

Probar: Enviar "Read 2401.04088 -- what's the main contribution?" para verificar la función de lectura de papers.

### 4. Configurar prompt de escritura de papers

```text
Help me write a research paper in LaTeX. Here's my workflow:

1. Start from the IEEE template (or article/beamer depending on what I need)
2. When I describe a section, generate the LaTeX source for it
3. After each major edit, compile and preview the PDF so I can check formatting
4. If there are compilation errors, read the log and fix them automatically
5. When I provide BibTeX entries, add them to the bibliography and recompile

Use xelatex if I need Chinese/CJK support, otherwise default to pdflatex.
Always run 2 passes for cross-references.
```

Probar: Enviar "Start a new IEEE paper titled 'A Survey of LLM Agents'. Give me the template with abstract and introduction sections filled in, then compile it." para verificar funciones de escritura y compilación.

## Consejos prácticos

- **Primero leer luego escribir**: Usar arxiv-reader para investigar papers relacionados, aclarar el hilo de investigación antes de comenzar a escribir, el agente puede ayudarte a mantener notas de lectura
- **Capítulos primero que texto completo**: Usar `arxiv_sections` para navegar primero la estructura, localizar precisamente los capítulos que necesitan lectura profunda, evitar perder tiempo en contenido no relevante
- **Escaneo rápido de resúmenes para filtrado de literatura**: Después de obtener un lote de IDs de papers candidatos, primero obtener resúmenes por lotes para filtrado rápido, luego leer profundamente uno por uno los papers que pasaron el filtrado
- **Dejar errores de compilación al agente**: Cuando reporte error de compilación de LaTeX, no leas el registro tú mismo, deja que el agente analice el error y repare automáticamente
- **Referencias cruzadas necesitan dos compilaciones**: Cuando involucre citas, referencias, numeración de figuras/tablas, el agente ejecutará automáticamente dos compilaciones para asegurar que las citas sean correctas

## Adaptación para usuarios de China

Los escenarios académicos nacionales tienen algunas diferencias con el extranjero: la escritura de papers en chino necesita soporte del paquete ctex + compilador xelatex, extraer imágenes de Docker necesita configurar fuentes nacionales, las tesis de universidades tienen requisitos de plantillas específicas. Los siguientes son esquemas de adaptación para estos escenarios.

### Configuración de escritura LaTeX en chino

Al usar la función de compilación LaTeX de Prismer para escribir papers en chino, necesitas especificar el compilador xelatex y cargar el paquete ctex. Informar al agente en el prompt:

```text
Necesito escribir paper en chino. Por favor nota:
1. Compilador usar xelatex (no pdflatex)
2. Cargar paquete ctex en la cabecera del documento: \usepackage[UTF8]{ctex}
3. Fuente de caracteres chinos usar esquema自带 del sistema (fontset=auto deja que ctex seleccione automáticamente)
4. Si necesitas fuentes personalizadas, usar \setCJKmainfont{} para configurar

Cada compilación usar xelatex, y ejecutar dos compilaciones para asegurar que el índice y las citas sean correctas.
```

Un ejemplo de plantilla mínima de LaTeX en chino:

```latex
\documentclass[12pt,a4paper]{article}
\usepackage[UTF8]{ctex}          % Paquete central para soporte de chino
\usepackage{geometry}
\geometry{left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm}
\usepackage{amsmath,amssymb}     % Fórmulas matemáticas
\usepackage{graphicx}            % Imágenes
\usepackage{hyperref}            % Hipervínculos
\usepackage[backend=biber,style=gb7714-2015]{biblatex}  % Formato de referencias en chino

\title{Título del paper}
\author{Nombre del autor}
\date{\today}

\begin{document}
\maketitle
\begin{abstract}
Contenido del resumen.
\end{abstract}

\section{Introducción}
Contenido del texto.

\printbibliography
\end{document}
```

> **Nota**: El contenedor Docker de Prismer incluye el entorno completo de TeX Live, ya tiene incorporado el paquete ctex y fuentes chinas comunes (colección de fuentes Fandol). Si necesitas usar fuentes del sistema (como Songti, Heiti), necesitas montar el directorio de fuentes en el contenedor Docker.

### Plantillas de tesis de universidades nacionales

La demanda más común en escenarios académicos chinos es la composición de tesis de grados. Las siguientes son plantillas LaTeX de universidades principales, todas open source y activamente mantenidas:

| Universidad | Nombre de plantilla | Dirección GitHub | Descripción |
|------|---------|-------------|------|
| Universidad de Tsinghua | ThuThesis | [tuna/thuthesis](https://github.com/tuna/thuthesis) | Plantilla de tesis de la Universidad de Tsinghua, mantenida por TUNA |
| Academia de Ciencias de China | ucasthesis | [mohuangrui/ucasthesis](https://github.com/mohuangrui/ucasthesis) | Plantilla de tesis de la Academia de Ciencias de China |
| Universidad de Zhejiang | zjuthesis | [TheNetAdmin/zjuthesis](https://github.com/TheNetAdmin/zjuthesis) | Soporta licenciatura, maestría, doctorado, disponible en Overleaf |
| Universidad de Ciencia y Tecnología de China | ustcthesis | [ustctug/ustcthesis](https://github.com/ustctug/ustcthesis) | Plantilla de tesis de la USTC |
| Más plantillas | latex-templates | [hantang/latex-templates](https://github.com/hantang/latex-templates) | Colección completa de plantillas LaTeX de universidades nacionales |

**Modo de uso**: Después de descargar la plantilla, puedes dejar que el agente de OpenClaw llene el contenido y compile basado en la estructura de la plantilla:

```text
Voy a escribir tesis de maestría, usar plantilla ThuThesis de la Universidad de Tsinghua.
Los archivos de plantilla están en el directorio ~/thesis/. Por favor:
1. Leer la estructura de la plantilla, entender la función de cada archivo
2. Generar contenido de cada capítulo según mi descripción
3. Compilar y previsualizar, asegurar que el formato cumpla con los requisitos de la plantilla
```

### Configuración de espejo de Docker nacional

La imagen Docker de Prismer incluye el entorno completo de TeX Live, el volumen de la imagen es grande. Se recomienda configurar aceleración de espejo de imágenes Docker para usuarios nacionales al extraer imágenes:

Configurar acelerador de espejo de imágenes Docker (las estaciones de espejo de Docker Hub nacionales cambian rápidamente, por favor buscar "aceleración de espejo de Docker nacional" para obtener direcciones disponibles más recientes):

Editar `/etc/docker/daemon.json` (crear si no existe):

```json
{
  "registry-mirrors": [
    "https://your-mirror.example.com"
  ]
}
```

Reiniciar servicio Docker:

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

**Cambio de fuente del gestor de paquetes de TeX Live (tlmgr)**:

Si necesitas instalar paquetes LaTeX adicionales dentro del contenedor, puedes cambiar la fuente CTAN al espejo de Tsinghua:

```bash
tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet
```

> 💡 También puedes usar el servicio de espejo de contenedores de Alibaba Cloud (necesita registro de cuenta para obtener dirección de aceleración exclusiva).
>
> **Usuarios de macOS**: La aceleración de imágenes de Docker Desktop para macOS se configura en Docker Desktop > Settings > Docker Engine, agregar el mismo campo `registry-mirrors`.

### Descripción de bases de datos de papers nacionales

La función de lectura de papers de este caso de uso depende de la API pública de arXiv, la siguiente es la descripción de compatibilidad de bases de datos académicas nacionales:

| Base de datos | ¿Disponible? | Descripción |
|--------|:-------:|------|
| **arXiv** | Disponible | Proporciona API pública gratuita, no necesita registro, cobertura completa de papers en áreas de AI/ML/CS |
| **Semantic Scholar** | Disponible | Proporciona API gratuita, puede servir como fuente complementaria de descubrimiento de papers |
| **CNKI (知网)** | No disponible | No proporciona API pública, el acceso al contenido necesita autorización institucional |
| **Wanfang Data (万方数据)** | No disponible | No proporciona API pública, acceso a través de instituciones como bibliotecas universitarias |
| **Weipu (维普)** | No disponible | No proporciona API pública |

**Impacto práctico**: Para investigadores en AI, aprendizaje automático, ciencias de la computación, arXiv ya cubre la mayoría de los papers importantes. Los papers en chino (como los publicados en CNKI, Wanfang) actualmente no se pueden procesar a través del flujo de trabajo automatizado de este caso de uso, necesitan descargarse manualmente y luego analizarse usando la función de lectura de archivos de OpenClaw.

### Comparación de plataformas LaTeX en línea nacionales

Si no quieres usar Docker para desplegar entorno LaTeX local, las siguientes son plataformas de edición LaTeX en línea disponibles nacionalmente:

> Las siguientes son plataformas LaTeX en línea disponibles nacionalmente, solo para referencia, no recomendación. Por favor elige según tus propias necesidades.

| Plataforma | Características | Costo | Escenario aplicable |
|------|------|------|---------|
| [Overleaf](https://www.overleaf.com) Versión internacional | Función más completa, ecosistema de plantillas más rico | La versión gratuita tiene límite de tiempo de compilación; versión de pago (ver [sitio oficial de Overleaf](https://www.overleaf.com/user/subscription/plans)) | Envío a revistas internacionales, colaboración con socios en el extranjero |
| [TeXPage](https://www.texpage.com) | Desarrollado por equipo nacional, cifrado de extremo a extremo, servidores en el país | De pago | Escenarios sensibles a seguridad de datos y latencia de red |
| [LoongTeX](https://www.loongtex.com) | Plataforma nacional, corrección de errores asistida por AI, soporta colaboración asíncrona | Proporciona versión gratuita | Colaboración en equipo de universidades nacionales |

**Comparación con el esquema de Prismer**: La ventaja de las plataformas en línea es cero configuración, pero no soportan el flujo de trabajo automatizado de escritura y compilación a través del agente de OpenClaw. La combinación de Prismer + OpenClaw es adecuada para usuarios que necesitan completar el flujo de trabajo automatizado completo de "leer papers relacionados -> escribir -> compilar -> modificar" en una conversación.

### Grupos aplicables

- **Investigadores de AI/ML**: Necesitan leer nuevos papers de arXiv diariamente, escribir papers de revistas/conferencias en inglés
- **Remitentes de revistas internacionales**: Necesitan usar plantillas LaTeX específicas como IEEE, ACM
- **Estudiantes de posgrado**: Necesitan usar plantillas designadas por la universidad para escribir tesis
- **Escritores cross-idioma**: Papers bilingües en chino e inglés (como citar literatura en inglés en tesis en chino)

## Enlaces relacionados

- [Prismer - Plataforma de investigación open source](https://github.com/Prismer-AI/Prismer) -- Proporciona skills arxiv-reader y latex-compiler
- [Skill arxiv-reader](https://github.com/Prismer-AI/Prismer/tree/main/skills/arxiv-reader) -- Herramienta de lectura de papers (3 herramientas)
- [Skill latex-compiler](https://github.com/Prismer-AI/Prismer/tree/main/skills/latex-compiler) -- Herramienta de compilación LaTeX (4 herramientas)
- [Documentación de API de arXiv](https://info.arxiv.org/help/api/) -- API oficial de arXiv
- [Documentación del paquete ctex](https://ctan.org/pkg/ctex) -- Paquete central de composición LaTeX en chino
- [ThuThesis - Plantilla de tesis de la Universidad de Tsinghua](https://github.com/tuna/thuthesis)
- [Colección de plantillas LaTeX de universidades nacionales](https://github.com/hantang/latex-templates) -- Incluye plantillas de tesis de cada universidad
- [Espejo CTAN de Tsinghua](https://mirrors.tuna.tsinghua.edu.cn/CTAN/) -- Fuente de espejo nacional de TeX Live
- [TeXPage](https://www.texpage.com) -- Plataforma de edición LaTeX en línea nacional
- [LoongTeX](https://www.loongtex.com) -- Plataforma de colaboración LaTeX nacional

---

**Enlaces originales**:
- [Lector de Papers arXiv (Inglés)](https://github.com/hesamsheikh/awesome-openclaw-usecases/pull/53)
- [Escritura de Papers LaTeX (Inglés)](https://github.com/hesamsheikh/awesome-openclaw-usecases/pull/53)
