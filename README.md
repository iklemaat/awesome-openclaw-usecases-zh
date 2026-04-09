<div align="center">

<img width="1500" height="500" alt="OpenClaw AI Agent Best Use Cases & Examples - 46 Real Scenarios" src="https://github.com/user-attachments/assets/4ae57dfb-4f18-4677-9136-43bf93017250" />

<br/>
<br/>

<p><strong>46 escenarios reales verificados, te enseñamos cómo automatizar trabajo y vida con agentes de IA</strong></p>

<br/>

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
![用例数量](https://img.shields.io/badge/Casos-46-blue?style=flat-square)
![中文](https://img.shields.io/badge/Idioma-Español-red?style=flat-square)
![新手友好](https://img.shields.io/badge/Nivel-Principiantes-green?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)
[![GitHub](https://img.shields.io/badge/Repo-Principal-GitHub-black?style=flat-square&logo=github)](https://github.com/AlexAnys/awesome-openclaw-usecases-zh)
[![AtomGit](https://img.shields.io/badge/Hosting-China-AtomGit-orange?style=flat-square)](https://atomgit.com/alex_anys/awesome-openclaw-usecases-zh)

**[Inicio Rápido](#-guía-de-inicio-rápido)** · **[Directorio de Casos](#-directorio)** · **[Guía de Contribución](CONTRIBUTING.md)**

</div>

---

# Awesome OpenClaw Colección de Mejores Casos de Uso (Versión Español) 🇪🇸

> Resolviendo el cuello de botella en la adopción de OpenClaw: no son las ~~habilidades~~, sino encontrar **formas de mejorar tu vida**.
>
> Una colección de casos de uso reales de OpenClaw para usuarios hispanohablantes, incluye adaptación de casos internacionales verificados por la comunidad y casos originales del ecosistema local, desde cero.

<details>
<summary><strong>📖 Declaración de Fuentes</strong></summary>

Este repositorio está inspirado en [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases), desarrollado independientemente sobre esa base, incluye muchos casos originales y adaptaciones del ecosistema local. Gracias al autor original [hesamsheikh](https://github.com/hesamsheikh) y a todos los contribuyentes de la comunidad.

</details>

> **Actualización 2026.3.30**: Añadidos 5 nuevos casos: Consola Omnipotente de Feishu (Lark CLI oficial), Monitor de Mercado de Acciones A, Análisis de Competencia, Descubrimiento de Papers en HuggingFace, Escritura con arXiv+LaTeX. Los casos adaptados localmente se expandieron a 19. Ver [Casos Adaptados Localmente](#-casos-adaptados-localmente).

---

## 🗂 Directorio

- [Guía de Inicio Rápido](#-guía-de-inicio-rápido)
- [Casos Adaptados Localmente](#-casos-adaptados-localmente) — Casos seleccionados adaptados al ecosistema local (Feishu, DingTalk, WeCom, AKShare, etc.)
- [Redes Sociales](#redes-sociales) — Obtener automáticamente contenido seleccionado de Reddit, YouTube, X y otras plataformas
- [Creatividad y Construcción](#creatividad-y-construcción) — Dejar que la IA cree contenido y construya aplicaciones para ti
- [Infraestructura y DevOps](#infraestructura-y-devops) — Auto-reparación de servidores, automatización de flujos de trabajo
- [Productividad](#productividad) — Organización de correos, gestión de agenda, atención al cliente, asistente personal
- [Investigación y Aprendizaje](#investigación-y-aprendizaje) — Base de conocimientos, investigación de mercados, análisis de competencia, descubrimiento de papers
- [Finanzas y Trading](#finanzas-y-trading) — Trading simulado en mercados de predicción

---

## 🚀 Guía de Inicio Rápido

[OpenClaw](https://github.com/openclaw/openclaw) (anteriormente ClawdBot, MoltBot) es un **agente de IA personal** de código abierto. A diferencia de los chats de IA convencionales, puede ejecutar tareas activamente, recordar permanentemente tus preferencias, conectarse a múltiples plataformas y ejecutarse automáticamente según un horario. En resumen, es un **empleado de IA en línea 24/7**. [Ver tutorial oficial en español](https://docs.openclaw.ai/es)

<details>
<summary><strong>🚀 Comenzar en tres pasos (clic para expandir)</strong></summary>

```
1️⃣  Instalar OpenClaw → github.com/openclaw/openclaw
2️⃣  Elegir un caso de uso interesante de abajo, o conectar tu Telegram / Discord / Feishu / DingTalk / WeCom
3️⃣  Copiar el prompt del caso de uso, pegarlo a tu OpenClaw
```

Así de simple.

</details>

<details>
<summary><strong>📖 Conceptos clave fáciles de entender (clic para expandir)</strong></summary>

| Concepto | Inglés | Explicación en una frase | Analogía cotidiana |
|:---:|:---:|---|---|
| Espacio de Trabajo | Workspace | El "hogar" del agente, todos los archivos están aquí | Como el escritorio de un empleado |
| Alma | SOUL.md | Define la personalidad, tono y límites del agente | Como la personalidad y valores de una persona |
| Manual de Operaciones | AGENTS.md | Le dice al agente cómo trabajar y cómo recordar | Como una descripción de puesto |
| Memoria | Memory | Recuerda lo que has dicho y tus preferencias | Como un asistente antiguo que te entiende mejor |
| Habilidad | Skill | Paquete de conocimientos que enseña al agente a hacer algo bien | Como trucos de experiencia que transmite un maestro |
| Herramienta | Tool | Capacidad concreta que puede invocar el agente (leer archivos, buscar, enviar mensajes, etc.) | Como herramientas de oficina al alcance del empleado |
| Canal | Channel | Conexión a plataformas como Telegram, Feishu, Discord, etc. | Como múltiples formas de contacto de un empleado |
| Prompt | Prompt | La instrucción que le das | Como asignar trabajo a un empleado |
| Tarea Programada | Cron Job | Ejecución automática según horario | Como configurar una alarma |
| Latido | Heartbeat | Verificación periódica automática del estado e informe proactivo | Como patrulla de seguridad |
| Sub-agente | Sub-agent | Enviar "alter ego" para procesamiento paralelo | Como liderar un pequeño equipo |
| Contexto | Context | Toda la información que el agente puede "ver" actualmente | Como memoria de trabajo, documentos extendidos sobre la mesa |
| Nodo | Node | Conectar teléfono/tableta como "ojos y manos" del agente | Como proporcionar un teléfono al empleado |

</details>

### ¿Cómo leer esta colección?

Cada caso de uso está organizado con un formato uniforme:

| Sección | Contenido |
|------|------|
| **Dolor** | ¿Qué problema resuelve este caso de uso? |
| **Qué puede hacer** | Lista de funciones específicas |
| **Habilidades requeridas** | ¿Qué plugins necesitas instalar? |
| **Cómo configurar** | Te enseña paso a paso la configuración (incluye prompts copiables) |
| **Consejos prácticos** | Errores comunes y mejores prácticas |

> 💡 **Consejo**: Los prompts en inglés en los bloques de código se recomiendan copiar y usar directamente para mejor efecto. Cada prompt tiene explicación en español arriba para ayudarte a entender su propósito.

> 🤖 **Amigable para Agentes**: El formato de esta colección está diseñado estructuralmente, además de facilitar tu lectura, también puedes intentar entregar el archivo del caso de uso a un agente de IA para ayudar a ejecutar los pasos de configuración. Este es un enfoque experimental, el efecto varía según la complejidad del caso — ver explicación específica en [AGENT-GUIDE.md](AGENT-GUIDE.md).

---

> ⚠️ **Recordatorio de Seguridad**: Las habilidades y dependencias de terceros citadas en esta colección **no han sido auditadas por los mantenedores**. Por favor revisa el código fuente, verifica permisos antes de usar, evita credenciales hardcodeadas.

> Explicación de dificultad: ⭐ Copiar y pegar para usar · ⭐⭐ Requiere configuración simple · ⭐⭐⭐ Requiere cierta base técnica

---

## 🌍 Casos Adaptados Localmente

> Casos diseñados o adaptados para el ecosistema de herramientas local, usando Feishu, DingTalk, WeCom, Xiaohongshu y otras plataformas locales. Los casos marcados como "adaptados" añaden soluciones locales sobre la versión internacional.

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Asistente IA de Feishu](usecases/cn-feishu-ai-assistant.md) | Desplegar OpenClaw como bot de Feishu, activar tareas de IA directamente en el chat, soporta automatización de documentos | ⭐⭐ |
| [Consola Omnipotente de Feishu (Lark CLI)](usecases/cn-feishu-lark-cli.md) | Dejar que el Agente de IA opere Feishu como tú — buscar documentos, leer Meetings, ver calendario, enviar mensajes, cubre 200+ comandos en 11 áreas | ⭐ |
| [Asistente IA de DingTalk](usecases/cn-dingtalk-ai-assistant.md) | Desplegar OpenClaw como bot de DingTalk, modo Stream sin necesidad de IP pública | ⭐⭐ |
| [Asistente IA de WeCom](usecases/cn-wecom-ai-assistant.md) | Usar IA en WeCom, plugin de WeChat permite usar también en WeChat personal | ⭐⭐ |
| [Resumen Matutino (adaptado)](usecases/custom-morning-brief.md) | Resumen diario programado a Feishu/DingTalk, soporta fuentes de noticias en español y configuración cron | ⭐ |
| [Suite de Automatización de Oficina](usecases/cn-office-automation.md) | Gestión de correos, organización de archivos, actas de reuniones, generación de informes semanales, soporta 163/QQ/Outlook | ⭐⭐ |
| [Automatización de Actas y Tareas de Reuniones (adaptado)](usecases/meeting-notes-action-items.md) | Transcripción de reuniones genera automáticamente actas y crea tareas (Feishu Meetings/Tencent Meetings/DingTalk) | ⭐⭐ |
| [Servicio al Cliente IA Multicanal (adaptado)](usecases/multi-channel-customer-service.md) | Automatización de servicio al cliente multicanal WeCom/Douyin/Xiaohongshu | ⭐⭐⭐ |
| [Automatización de Contenido de Xiaohongshu](usecases/cn-xiaohongshu-automation.md) | Automatización de flujo completo desde tema, copy, portada hasta publicación programada | ⭐ |
| [Línea de Producción de Podcasts (adaptado)](usecases/podcast-production-pipeline.md) | Automatización de producción de podcasts de principio a fin (Xiaoyu/Ximalaya/Bilibili) | ⭐⭐ |
| [Rastreador de Informes Financieros IA (adaptado)](usecases/earnings-tracker.md) | Seguimiento de informes financieros de acciones A, fuente de datos gratuita AKShare + automatización de previsiones/anuncios | ⭐⭐ |
| [Validador de Ideas Pre-Desarrollo (adaptado)](usecases/pre-build-idea-validator.md) | Escaneo automático de competencia antes de codificar (Índice Baidu/Índice WeChat/V2EX/Minority) | ⭐⭐ |
| [Sistema Operativo de Colaboración Multi-Agente](usecases/cn-multi-agent-operating-system.md) | Convertir OpenClaw en un sistema de agentes con división profesional, colaboración y iteración estable | ⭐⭐⭐ |
| [Equipo de Desarrollo de Una Persona Agent Swarm (adaptado)](usecases/agent-swarm-dev-team.md) | OpenClaw orquesta flota Codex + Claude Code, pipeline de desarrollo totalmente automatizada | ⭐⭐⭐ |
| [Arquitectura Multi-Agente de E-commerce](usecases/cn-ecommerce-multi-agent.md) | Multi-agente vinculado a grupo Feishu implementa división de roles en e-commerce, 4 Skills cubren ventas/inventario/cliente | ⭐⭐ |
| [Monitor Diario de Mercado de Acciones A](usecases/cn-a-share-monitor.md) | Resumen pre-apertura + revisión post-cierre + flujo de fondos por sector, fuente de datos AKShare/MCP | ⭐⭐ |
| [Análisis de Competencia y Monitoreo de Precios (adaptado)](usecases/competitive-intelligence.md) | Informe semanal de competencia con Perplexity + Firecrawl MCP, Índice Baidu/Índice WeChat/push Feishu | ⭐⭐ |
| [Descubrimiento de Papers de HuggingFace (adaptado)](usecases/hf-papers-research-discovery.md) | Filtrado diario de papers ML populares + lectura profunda de arXiv, espejo HF/push Feishu | ⭐ |
| [Lectura de Papers arXiv y Escritura LaTeX (adaptado)](usecases/arxiv-paper-reader-latex-writer.md) | Obtención de papers/navegación por capítulos/resumen rápido + compilación instantánea LaTeX, plantilla en español/adaptación imagen Docker | ⭐⭐ |

---

## Redes Sociales

> Agregar automáticamente contenido que te importa, despedirse de la ansiedad de información

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Resumen Diario de Reddit](usecases/daily-reddit-digest.md) | Generar resumen seleccionado de tus subreddits favoritos según tus preferencias | ⭐ |
| [Resumen Diario de YouTube](usecases/daily-youtube-digest.md) | Obtener resumen de nuevos videos diarios de tus canales seguidos, no te pierdas ningún contenido | ⭐ |
| [Análisis de Cuenta de X](usecases/x-account-analysis.md) | Obtener análisis cualitativo de tu cuenta de X (antes Twitter) | ⭐⭐ |
| [Resumen de Noticias Tecnológicas Multifuente](usecases/multi-source-tech-news-digest.md) | Agregar automáticamente noticias tecnológicas de 109+ fuentes, soporta calificación de calidad y distribución multicanal | ⭐⭐ |

## Creatividad y Construcción

> Dejar que los agentes de IA te ayuden a crear contenido y construir productos desde cero

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Tareas Autónomas Dirigidas por Objetivos](usecases/overnight-mini-app-builder.md) | Decirle a la IA tu objetivo, ella lo desglosa y ejecuta diariamente, puede crear mini-apps en una noche | ⭐⭐ |
| [Línea de Producción de Contenido de YouTube](usecases/youtube-content-pipeline.md) | Automatizar descubrimiento de ideas de videos, investigación y seguimiento para canal de YouTube | ⭐⭐⭐ |
| [Fábrica de Contenido Multi-Agente](usecases/content-factory.md) | Ejecutar línea de producción de contenido con tres agentes (investigación, escritura, diseño) en Discord | ⭐⭐⭐ |

## Infraestructura y DevOps

> Dejar que los servidores se reparen solos, tú solo duerme

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Orquestación de Flujo de Trabajo n8n](usecases/n8n-workflow-orchestration.md) | Delegar llamadas API a flujos de trabajo n8n via webhook, el agente nunca toca credenciales | ⭐⭐⭐ |
| [Observabilidad Opik](usecases/opik-openclaw-observability.md) | Conectar enlaces de ejecución de OpenClaw a Opik, ver seguimiento unificado de LLM/herramientas/sub-agentes, monitorear tokens y costos | ⭐⭐ |
| [Servidor Casero Auto-Reparable](usecases/self-healing-home-server.md) | Ejecutar agente de infraestructura siempre en línea, detectar y reparar fallos automáticamente | ⭐⭐⭐ |
| [Equipo de Desarrollo de Una Persona Agent Swarm](usecases/agent-swarm-dev-team.md) | OpenClaw orquesta flota Codex + Claude Code implementa pipeline de desarrollo totalmente automatizada (adaptación local) | ⭐⭐⭐ |

## Productividad

> Dejar trabajos repetitivos a la IA, concentrarse en lo realmente importante

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Resumen Matutino Personalizado](usecases/custom-morning-brief.md) | Resumen exclusivo al despertar — noticias, tareas, borradores de contenido, operaciones recomendadas por IA (adaptación local) | ⭐ |
| [Limpieza de Bandeja de Entrada](usecases/inbox-declutter.md) | Resumir automáticamente newsletters y enviar resúmenes por correo, despedirse de correos acumulados | ⭐ |
| [Segundo Cerebro](usecases/second-brain.md) | Enviar mensajes para recordar todo, buscar en cualquier momento en panel personalizado | ⭐ |
| [CRM Personal](usecases/personal-crm.md) | Descubrir y rastrear contactos automáticamente desde correos y calendario, soporta consultas en lenguaje natural | ⭐⭐ |
| [Rastreador de Salud y Síntomas](usecases/health-symptom-tracker.md) | Rastrear ingesta de alimentos y síntomas para identificar desencadenantes de alergias, con recordatorios periódicos de check-in | ⭐ |
| [Asistente Personal Basado en Teléfono](usecases/phone-based-personal-assistant.md) | Acceder a tu agente de IA desde cualquier teléfono vía llamada o SMS | ⭐⭐ |
| [Asistente Personal Multicanal](usecases/multi-channel-assistant.md) | Un asistente de IA gestiona Telegram, Slack, correo y calendario unificados | ⭐⭐ |
| [Asistente de Calendario Familiar y Tareas Domésticas](usecases/family-calendar-household-assistant.md) | Agregar todos los calendarios familiares al resumen matutino, monitorear mensajes para citas, gestionar inventario del hogar | ⭐⭐ |
| [Gestor de Tareas Todoist](usecases/todoist-task-manager.md) | Sincronizar razonamiento de IA y registro de progreso a Todoist, ver en tiempo real qué está haciendo el agente | ⭐⭐ |
| [Servicio al Cliente IA Multicanal](usecases/multi-channel-customer-service.md) | WhatsApp + Instagram + correo + reseñas de Google unificados en bandeja de entrada de IA (adaptación local) | ⭐⭐⭐ |
| [Confirmación de Invitados a Eventos](usecases/event-guest-confirmation.md) | Llamar automáticamente a invitados uno por uno para confirmar asistencia, recopilar notas y compilar resumen | ⭐⭐ |
| [Gestión de Estado de Proyectos](usecases/project-state-management.md) | Seguimiento de proyectos impulsado por eventos, capturar contexto automáticamente, reemplazar tableros estáticos | ⭐⭐⭐ |
| [Panel Dinámico](usecases/dynamic-dashboard.md) | Panel en tiempo real, sub-agentes obtienen datos en paralelo desde API, base de datos y redes sociales | ⭐⭐⭐ |
| [Gestión Autónoma de Proyectos](usecases/autonomous-project-management.md) | Coordinar proyectos multi-agente usando patrón STATE.yaml, sin orquestación humana | ⭐⭐⭐ |
| [Equipo Profesional Multi-Agente](usecases/multi-agent-team.md) | 4 agentes de IA profesionales (estrategia + desarrollo + marketing + negocios) como tu equipo virtual | ⭐⭐⭐ |

## Investigación y Aprendizaje

> Dejar que la IA te ayude a organizar conocimiento, rastrear mercados y descubrir oportunidades

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Base de Conocimientos Personal (RAG)](usecases/knowledge-base-rag.md) | Arrastrar URLs, tweets y artículos al chat, construir base de conocimientos con búsqueda semántica | ⭐⭐ |
| [Rastreador de Informes Financieros IA](usecases/earnings-tracker.md) | Rastrear informes financieros de empresas tecnológicas/IA, automatizar vistas previas, recordatorios y resúmenes detallados (adaptación local) | ⭐⭐ |
| [Búsqueda Semántica de Memoria](usecases/semantic-memory-search.md) | Agregar búsqueda semántica impulsada por vectores a archivos de memoria de OpenClaw | ⭐⭐ |
| [Investigación de Mercados y Fábrica de Productos](usecases/market-research-product-factory.md) | Extraer dolores reales de Reddit y X, dejar que la IA construya soluciones MVP | ⭐⭐⭐ |
| [Validador de Ideas Pre-Desarrollo](usecases/pre-build-idea-validator.md) | Escaneo automático de competencia antes de codificar, devuelve puntuación de competitividad (adaptación local) | ⭐⭐ |
| [Análisis de Competencia y Monitoreo de Precios](usecases/competitive-intelligence.md) | Informe semanal de competencia con Perplexity + Firecrawl MCP, alternativa de $1.20/mes a suscripción de $150 (adaptación local) | ⭐⭐ |
| [Descubrimiento de Papers de HuggingFace](usecases/hf-papers-research-discovery.md) | Filtrado diario de papers ML populares, navegación de metadatos, señales de comunidad + lectura profunda de arXiv (adaptación local) | ⭐ |
| [Lectura de Papers arXiv y Escritura LaTeX](usecases/arxiv-paper-reader-latex-writer.md) | Obtención de papers/navegación por capítulos/resumen rápido + compilación instantánea LaTeX, sin entorno TeX local (adaptación local) | ⭐⭐ |

## Finanzas y Trading

> Usar IA para trading simulado y backtesting de estrategias en mercados de predicción

| Nombre | Descripción | Dificultad |
|------|------|:---:|
| [Piloto Automático de Polymarket](usecases/polymarket-autopilot.md) | Trading simulado automatizado en mercados de predicción, con backtesting, análisis de estrategias e informes diarios de rendimiento | ⭐⭐⭐ |



---

> 💡 Si puedes usar bien estos casos de uso de OpenClaw, significa que estás muy avanzado en colaboración con IA. Detrás de estas herramientas está el rápido desarrollo de capacidades de ingeniería de Coding Agents. Para aprovechar más libremente esta capacidad y crear aplicaciones personalizadas de nivel productivo, sigue **[Agent-First Development](https://github.com/AlexAnys/agent-first-dev)** — diseñado para estudiantes de todos los orígenes basado en autorizaciones de profesores de Stanford y otros. Proyectos en desarrollo, comentarios valiosos son bienvenidos.

## 🌟 ¿Por qué elegir esta colección?

| | Aprender por tu cuenta | Ver tutoriales fragmentados | **Esta colección** |
|---|:---:|:---:|:---:|
| Casos verificados reales | ❌ Incierto si funcionan | ⚠️ Calidad variable | ✅ Cada caso tiene fuentes respaldadas |
| Adaptación al ecosistema local | ❌ Necesitas reemplazar herramientas tú mismo | ❌ Mayormente soluciones extranjeras | ✅ Feishu/DingTalk/WeCom/Xiaohongshu |
| Te ayuda a elegir dirección correcta | ❌ Demasiados skills, no sabes por dónde empezar | ⚠️ Solo hablan de funciones, no de escenarios | ✅ Organizado por escenarios reales, dificultad graduada |
| Funciona siguiendo los pasos | ❌ Faltan configuración y prompts | ⚠️ Pasos a menudo incompletos | ✅ Pasos completos + prompts copiables |
| Desde cero para principiantes | ❌ Asume que ya sabes | ⚠️ Cada uno dice lo suyo | ✅ Conceptos clave explicados + guía para principiantes |

---

## 🤝 Contribuir

Damos la bienvenida a casos de uso verificados reales. Ver [CONTRIBUTING.md](CONTRIBUTING.md).

- **Enviar casos del ecosistema local** (Feishu, DingTalk, WeCom, Xiaohongshu, etc.)
- **Adaptar casos internacionales** (añadir soluciones locales sobre casos existentes)
- **Mejorar contenido existente** (complementar prácticas, corregir información desactualizada)

> Criterios de inclusión: Ejecutado exitosamente + verificación multi-fuente + reproducibles siguiendo los pasos. No aceptamos casos no probados ni contenido relacionado con criptomonedas.

---

## 📌 Recursos Relacionados

- [Repositorio Oficial de OpenClaw](https://github.com/openclaw/openclaw) — Instalación y documentación oficial
- [Repositorio Principal en GitHub](https://github.com/AlexAnys/awesome-openclaw-usecases-zh) — Desarrollo principal y discusión aquí
- [Hosting en AtomGit China](https://atomgit.com/alex_anys/awesome-openclaw-usecases-zh) — Acceso más rápido en China, contenido sincronizado

---

<div align="center">

**¿Te fue útil? ¡Apoya con un ⭐ Star para ayudar a más usuarios hispanohablantes a descubrir esta colección!**

</div>

---

## 📈 Historial de Stars

<a href="https://star-history.com/#AlexAnys/awesome-openclaw-usecases-zh&Date">
 <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=AlexAnys/awesome-openclaw-usecases-zh&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=AlexAnys/awesome-openclaw-usecases-zh&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=AlexAnys/awesome-openclaw-usecases-zh&type=Date" />
  </picture>
</a>
