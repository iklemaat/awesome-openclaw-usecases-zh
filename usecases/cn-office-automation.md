# Suite de Automatización de Oficina

Los trabajadores del conocimiento gastan 2-3 horas diarias en tareas de oficina repetitivas: filtrar correos, organizar archivos, escribir actas de reuniones, compilar informes semanales. Cada uno individualmente no es difícil, pero en conjunto consumen mucho tiempo y energía.

Este caso de uso entrega las tareas repetitivas más comunes en oficina diaria a OpenClaw, para que las procese automáticamente según tus preferencias.

## Qué puede hacer

| Escenario | Efecto |
|------|------|
| **Gestión de correos** | Revisar bandeja de entrada periódicamente, clasificar automáticamente, resumir correos importantes, responder según reglas |
| **Organización de archivos** | Clasificar automáticamente archivos en carpeta de descargas por tipo/fecha/proyecto |
| **Actas de reuniones** | Generar automáticamente actas estructuradas después de reuniones, enviar a grupos de chat |
| **Generación de informes semanales** | Resumir correos de trabajo y avances de tareas de la semana, generar informe formateado |
| **Sincronización de agenda** | Extraer información de citas de capturas de pantalla de WeChat, crear eventos de calendario automáticamente |

## Habilidades requeridas

Según tu proveedor de correo:

- Correos nacionales de China (163/QQ): [imap-smtp-email](https://playbooks.com/skills/openclaw/skills/imap-smtp-email) — Soporta protocolo IMAP/SMTP estándar, compatible con servicios nacionales como 163, QQ, etc.
- Gmail: [gog](https://docs.openclaw.ai/tools/skills) — Skill de Google Workspace mantenido oficialmente por OpenClaw (necesita instalación y configuración OAuth)
- Outlook/Microsoft 365: [outlook](https://playbooks.com/skills/openclaw/skills/outlook) — Gestión completa de correo y calendario de Outlook (instalación: `npx playbooks add skill openclaw/skills --skill outlook`)

## Cómo configurar

### Automatización de correo (usando correo 163 como ejemplo)

1. Instalar skill de correo:
```bash
npx playbooks add skill openclaw/skills --skill imap-smtp-email
```

2. Configurar IMAP/SMTP de correo 163 (necesitas habilitar IMAP en configuración del correo y obtener código de autorización)

> **Consejo de seguridad**: El código de autorización del correo es información sensible, por favor configura a través del archivo `.env` del skill, no pegues directamente en conversaciones. Asegura que `.env` se agregue a `.gitignore`.

3. Configurar tarea programada:
```text
Revisa mi bandeja de entrada cada mañana a las 9, envíame un resumen de los correos importantes de las últimas 24 horas.
Reglas de filtrado: ignorar correos publicitarios y de suscripción, enfocar en correos de miembros del equipo y clientes.
Guardar el resumen en la memoria, conveniente para que lo vea en cualquier momento.
```

### Organización de archivos (referencia de prompt)

Las herramientas de sistema de archivos integradas de OpenClaw pueden operar archivos locales, solicitarán confirmación antes de ejecutar. Se recomienda probar primero en carpeta de prueba la primera vez:
```text
Ayúdame a clasificar los archivos en la carpeta de descargas por tipo:
- PDF poner en carpeta "Documentos"
- Imágenes poner en carpeta "Imágenes"
- Tablas poner en carpeta "Datos"
Archivos sin modificar por más de 30 días mover a carpeta "Archivado".
```

### Generación de informes semanales (referencia de prompt)

```text
Resume mis correos de trabajo enviados y recibidos esta semana, extrae avances clave de proyectos y tareas pendientes, genera un informe semanal.
Formato: Clasificar por proyecto, listar avances de esta semana y plan de la próxima semana para cada proyecto.
```

## Consejos prácticos

- **Automatización progresiva**: Comienza con un escenario (como resumen de correos), después de que funcione expande a otros escenarios
- **Configurar memoria de preferencias**: Deja que OpenClaw recuerde tus preferencias de clasificación de correos, formato de informes semanales, reglas de organización de archivos, optimiza gradualmente con el uso
- **Combinación de tareas programadas**: Múltiples tareas se pueden encadenar — 9 AM resumen de correos → 5 PM organizar archivos del día → Viernes 4 PM generar informe semanal

## Enlaces relacionados

- [Skill imap-smtp-email - ClawHub](https://playbooks.com/skills/openclaw/skills/imap-smtp-email)
- [Skill outlook - ClawHub](https://playbooks.com/skills/openclaw/skills/outlook)
- [Caso práctico de automatización de oficina con OpenClaw - CSDN](https://blog.csdn.net/weixin_41194129/article/details/157644237)
