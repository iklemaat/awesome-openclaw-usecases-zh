# Operar Feishu con Agentes usando Lark CLI

Chatear con IA en Feishu es solo el punto de partida. El mayor valor es: **dejar que la IA opere directamente todo en Feishu por ti** — buscar documentos, leer Minutes, ver calendario, enviar mensajes, modificar tablas multidimensionales, solo necesitas decir una frase en lenguaje natural.

Feishu lanzó oficialmente [Lark CLI](https://github.com/larksuite/cli) (MIT) el 2026-03-28, esta es una herramienta CLI (línea de comandos), cubre 11 áreas de negocio de Feishu, 200+ comandos, incluye 19 AI Agent Skills (paquetes de habilidades de agentes inteligentes) integrados. Después de instalar, tu Agente de IA puede operar Feishu como tu identidad.

## Qué puede hacer

| Área | Capacidad | Tú dices una frase |
|------|------|-----------|
| 💬 Mensajes | Enviar mensajes, crear grupos, buscar grupos, buscar mensajes | "Ayúdame a buscar el grupo del equipo del proyecto" |
| 📄 Documentos | Crear/leer/escribir/buscar documentos | "Busca documentos sobre OKR en Feishu" |
| 📊 Tablas multidimensionales | Tablas/campos/registros/vistas/dashboards | "Ayúdame a agregar un registro en la tabla multidimensional" |
| 📈 Hojas de cálculo | Crear/leer/escribir/agregar/exportar | "Extrae los datos de esta tabla" |
| 📅 Calendario | Consulta de agenda, crear eventos, consulta de ocupado/libre | "Ayúdame a ver qué agenda hay hoy" |
| 📧 Correo | Ver/buscar/enviar/responder correos | "Ver qué correos recientes hay en el correo de Feishu" |
| ✅ Tareas | Crear/consultar/completar tareas | "Ayúdame a crear una tarea de Feishu" |
| 📚 Base de conocimientos | Gestión de espacios/nodos | "Busca documentos en la base de conocimientos" |
| 👤 Directorio | Buscar personas por nombre/correo/teléfono | "Busca la información de contacto de Zhang San en Feishu" |
| 🎥 Reuniones/Minutes | Buscar grabaciones, ver actas de reuniones/transcripciones | "Busca los Minutes de ayer, extrae el contenido" |
| 📁 Espacio en la nube | Subir/bajar archivos, gestión de permisos | "Ayúdame a subir este archivo al espacio en la nube de Feishu" |

### Diferencia con el caso de uso "Asistente IA de Feishu"

| | Asistente IA de Feishu | Este caso de uso (Lark CLI) |
|---|---|---|
| **Qué hace** | Chatear con IA en Feishu | Dejar que la IA te ayude a operar Feishu |
| **Dirección** | Feishu → IA → Feishu (chat) | IA → Feishu (operación activa) |
| **Identidad** | Identidad de bot | Tu identidad personal (autorización abierta OAuth) |
| **Cobertura** | Principalmente envío/recepción de mensajes | 11 áreas de negocio, 200+ comandos |
| **Dependencia** | Necesita configurar plugin de Feishu | Ejecución independiente, no depende del plugin Feishu de OpenClaw |

**Los dos son complementarios**: El Asistente IA de Feishu te permite chatear con IA en Feishu, Lark CLI deja que la IA opere activamente todo en Feishu por ti. Se pueden usar simultáneamente.

## Habilidades requeridas

- [Lark CLI](https://github.com/larksuite/cli) (4,400+ stars, MIT, mantenido por Feishu oficial)
- Node.js / npm (necesario para instalar CLI y Agent Skills)
- Cuenta de Feishu (versión personal o empresarial, cualquiera funciona)

> Lark CLI incluye 19 Agent Skills, al instalar se symlink (enlace simbólico) automáticamente a marcos de Agentes como OpenClaw / Claude Code / Cursor, no necesita instalar skills adicionales de OpenClaw.

## Cómo configurar

### Instalar en una frase

Envía esta frase a tu Agente de IA:

```text
Ayúdame a instalar Feishu Lark CLI.
Consulta esta guía: https://github.com/AlexAnys/openclaw-feishu/blob/main/docs/lark-cli-guide.md
```

El Agente completará automáticamente la instalación y configuración, luego te dará un enlace de autorización. Abres el enlace en el navegador, confirmas con tu cuenta de Feishu — solo este paso.

> No necesitas crear una aplicación de Feishu por adelantado, CLI te ayudará a crearla automáticamente.

### Instalación manual (si quieres hacerlo tú mismo)

```bash
# Instalar
npm install -g @larksuite/cli

# Crear aplicación de Feishu y configurar (te dará un enlace de navegador, clic para confirmar)
lark-cli config init --new

# Autorización de inicio de sesión (también es un enlace de navegador, clic para confirmar)
lark-cli auth login --domain all

# Instalar AI Agent Skills
npx skills add larksuite/cli -y -g

# Verificar
lark-cli doctor
```

## Ejemplos de uso

Después de instalar, simplemente dile a tu IA. Los siguientes son escenarios verificados como usables (verificado 2026-03-30):

### Buscar documentos

```text
Tú: Busca documentos sobre "informe semanal" en Feishu
```

El Agente ejecuta `lark-cli docs +search --query "informe semanal" --format pretty`, devuelve lista de documentos (título, tipo, hora de modificación, enlace).

### Leer actas inteligentes de Minutes

```text
Tú: Busca los Minutes de ayer, extrae el contenido de las actas
```

El Agente primero busca `lark-cli docs +search --query "registro de texto"`, encuentra el documento Minutes y luego usa `lark-cli docs +fetch --doc "URL del documento"` para leer el contenido completo de las actas inteligentes — incluye resumen, capítulos, decisiones clave, frases destacadas.

### Ver calendario

```text
Tú: Ayúdame a ver qué agenda hay hoy
```

El Agente ejecuta `lark-cli calendar +agenda`, devuelve todos los eventos del día.

### Buscar grupos

```text
Tú: Ayúdame a buscar grupos llamados "equipo del proyecto" en Feishu
```

El Agente ejecuta `lark-cli im +chat-search --query "equipo del proyecto"`, devuelve nombre del grupo, ID del grupo, hora de creación, etc.

### Buscar personas

```text
Tú: Busca Zhang San en Feishu
```

El Agente ejecuta `lark-cli contact +search-user --query "Zhang San"`, devuelve nombre, departamento, avatar, etc.

## Consejos prácticos

- **Validez del token**: Access Token (token de acceso) 2 horas (renovación automática), Refresh Token (token de actualización) 7 días. Si pasan más de 7 días sin usar, necesitas volver a autorizar una vez — solo dile al Agente "volver a autorizar Feishu"
- **Formato de salida**: Para el Agente `--format json` (por defecto) es más fácil de analizar; para humanos `--format pretty` o `--format table` es más intuitivo
- **`--dry-run` (modo previsualización)**: Antes de ejecutar operaciones que pueden producir efectos secundarios (como enviar mensajes, modificar registros), puedes usar `--dry-run` para previsualizar la solicitud primero
- **`--page-all`**: Los resultados de consultas pueden estar paginados, agrega `--page-all` para obtener todo automáticamente
- **Coexistir con Asistente IA de Feishu**: Lark CLI usa identidad de usuario OAuth, no interfiere con la identidad de bot del plugin Feishu, se pueden usar simultáneamente

## Enlaces relacionados

- [Lark CLI GitHub](https://github.com/larksuite/cli) — Código fuente oficial y documentación completa
- [Guía de inicio de Lark CLI](https://github.com/AlexAnys/openclaw-feishu/blob/main/docs/lark-cli-guide.md) — Instalación, configuración, referencia técnica de Agentes
- [Plataforma abierta de Feishu](https://open.feishu.cn) — Documentación de API
- [openclaw-feishu](https://github.com/AlexAnys/openclaw-feishu) — Guía de configuración de Feishu × OpenClaw
