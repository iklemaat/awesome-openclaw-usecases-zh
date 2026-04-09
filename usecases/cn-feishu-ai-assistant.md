# Asistente IA de Feishu

Feishu es la principal herramienta de comunicación de muchos equipos, pero las capacidades de IA y el flujo de trabajo diario están separados — necesitas salir de Feishu para usar IA, y luego volver para pegar los resultados.

Este caso de uso despliega OpenClaw directamente como un bot de Feishu. Enviar mensajes en conversaciones de Feishu puede activar tareas de IA, y los resultados se responden directamente en la conversación, sin necesidad de cambiar ninguna herramienta.

> **Actualización 2026.3**: Feishu lanzó oficialmente un plugin de OpenClaw para Feishu, soporta operar documentos, calendario, tareas, etc. como usuario. Actualmente hay dos formas principales de conectar Feishu a OpenClaw, ver [sugerencias de selección](#sugerencias-de-selección-cómo-elegir-entre-los-dos-plugins) abajo.

## Qué puede hacer

- **Asistente IA conversacional**: Conversar directamente con OpenClaw en chats privados o grupales de Feishu
- **Automatización de tareas**: Activar procesamiento de archivos, consultas de información, recordatorios programados mediante mensajes de chat
- **Soporte multimedia**: Soporta envío y procesamiento de imágenes y archivos
- **Salida en streaming**: Las respuestas de IA se muestran carácter por carácter en tiempo real, no hay que esperar a que se genere todo el texto
- **Gestión de grupos**: Se puede configurar estrategia de @mención para activar, lista blanca/negra, etc.
- **Enrutamiento multi-agente**: Un bot de Feishu puede enrutar a diferentes agentes de IA según usuario/grupo

## Habilidades requeridas

Las funciones básicas de mensajería no requieren instalación adicional — OpenClaw 2026.2+ ya incluye el plugin de canal Feishu. Si necesitas que la IA opere documentos Feishu, calendario, tareas, etc., puedes usar el plugin oficial de Feishu (elegir uno de los dos, no se pueden habilitar simultáneamente, ver sugerencias de selección abajo).

## Sugerencias de selección: Cómo elegir entre los dos plugins

| Dimensión | Plugin oficial de Feishu | Plugin integrado de OpenClaw |
|---|---|---|
| **Identidad de operación** | Como **tu persona** (autorización OAuth) | Como **bot** |
| **Envío/recepción de mensajes** | ✅ | ✅ |
| **Operación de documentos** | ✅ Crear + editar + leer | Principalmente lectura |
| **Tablas multidimensionales** | ✅ Operación completa | Lectura/escritura básica |
| **Calendario / Tareas** | ✅ | ❌ |
| **Complejidad de instalación** | Necesita instalar herramienta CLI adicional + OAuth | Integrado y listo, `openclaw channels add` |
| **Mantenedor** | Equipo de Feishu | Comunidad OpenClaw |

**En resumen**: Usar principalmente Feishu como puerta de entrada de chat → elegir plugin integrado (simple); necesitar que la IA te ayude a operar documentos, crear tablas, agendar reuniones → elegir plugin oficial de Feishu (más funciones).

> ⚠️ **Los dos plugins son mutuamente excluyentes, solo se puede habilitar uno.** Al instalar el plugin oficial de Feishu, se deshabilitará automáticamente el plugin integrado. Si ambos están instalados, aparecerá un error `duplicate plugin id` haciendo que la función de Feishu no esté disponible. Si encuentras esto, elimina la copia en el directorio del usuario:
> ```bash
> rm -rf ~/.openclaw/extensions/feishu
> openclaw gateway restart
> ```
> Ver comparación detallada y guía de migración en [guía de la comunidad openclaw-feishu](https://github.com/AlexAnys/openclaw-feishu).

## Cómo configurar

> Los siguientes pasos aplican para **plugin integrado de OpenClaw** (plan b). Si eliges el plugin oficial de Feishu (plan a), por favor consulta [guía de instalación del plugin oficial de Feishu](https://github.com/AlexAnys/openclaw-feishu/blob/main/docs/feishu-official-plugin.md).
>
> ⚠️ **Problema conocido (2026-03-09, afecta OpenClaw ≥ 2026.3.x)**: La herramienta de instalación oficial de Feishu `feishu-plugin-onboard` tiene un bug en la verificación de versión ([openclaw-feishu#59](https://github.com/AlexAnys/openclaw-feishu/issues/59)), reportará erróneamente "versión demasiado baja". **Modo alternativo**: Saltar CLI onboard, ejecutar directamente `openclaw plugins install @larksuiteoapi/feishu-openclaw-plugin`, luego configurar manualmente appId/appSecret. Ver guía de instalación para detalles.

Por favor consulta la [guía de configuración de openclaw-feishu](https://github.com/AlexAnys/openclaw-feishu) para un tutorial completo paso a paso, a continuación un resumen de los pasos principales (aproximadamente 15-20 minutos):

### Primer paso: Crear aplicación de Feishu

Crear una aplicación auto-construida empresarial en [Backend de Desarrollador de Feishu](https://open.feishu.cn), habilitar capacidades de bot.

### Segundo paso: Configurar permisos

Importar permisos requeridos por lotes en "Gestión de Permisos" (`im:message`, `im:chat`, `im:resource`, etc.), el tutorial proporciona JSON completo de permisos que se puede importar con un clic.

### Tercer paso: Configurar suscripción de eventos

**Clave**: Seleccionar modo "Usar conexión de larga duración para recibir eventos" — así no se necesita IP pública ni nombre de dominio, tu computadora personal puede ejecutar.

Agregar evento: `im.message.receive_v1` (recibir mensajes)

### Cuarto paso: Conectar OpenClaw

```bash
# Agregar canal Feishu (guía interactiva)
openclaw channels add
# Elegir Feishu → pegar App ID → pegar App Secret

# Reiniciar gateway
openclaw gateway restart
```

### Quinto paso: Prueba y auto-inicio al encender

Buscar tu bot en Feishu, enviar mensaje para probar. Después de confirmar que funciona normalmente, configurar auto-inicio al encender:
```bash
openclaw gateway install
```

## Consejos prácticos

- **Modo de conexión larga es el preferido**: No necesita IP pública, no necesita dominio, no necesita servidor en la nube, solo computadora personal o NAS
- **Autorización emparejada**: El usuario primero recibirá un código de emparejamiento, el administrador debe aprobar en la terminal — esta es una característica de seguridad, no un fallo
- **Estrategia de grupo**: Se recomienda configurar "solo responder cuando @al bot", para evitar ser demasiado activo en grupos
- **Lark versión internacional**: Si usas Lark versión internacional en lugar de Feishu versión China, necesitas usar modo Webhook + Cloudflare Tunnel, el tutorial tiene explicación detallada

## Avanzado: Automatización de documentos y gestión de permisos

> **Si usas el plugin oficial de Feishu**: Las operaciones de documentos, tablas multidimensionales, calendario, tareas ya están integradas, no se necesitan los siguientes pasos de instalación adicionales.

Después de conectar el asistente IA básico de Feishu (usuarios del plugin integrado), puedes instalar adicionalmente skills relacionados con documentos de Feishu para lograr escenarios avanzados como "organizar actas en documento Feishu con una frase después de la reunión".

### Instalar skills de documentos de Feishu

```bash
npx playbooks add skill openclaw/openclaw --skill feishu-doc
npx playbooks add skill openclaw/openclaw --skill feishu-drive
npx playbooks add skill openclaw/openclaw --skill feishu-perm
```

| Skill | Capacidad |
|------|------|
| feishu-doc | Leer/escribir/crear/agregar documentos Feishu |
| feishu-drive | Gestión de archivos de nube Feishu |
| feishu-perm | Gestión de permisos de colaboradores de documentos/carpetas |

### Ejemplo de uso

Después de la reunión, enviar una frase:

```text
Organiza la discusión de la reunión de revisión de productos de esta tarde en un documento de Feishu, colócalo en la carpeta "Actas de Reunión".
Formato: Conclusiones → Elementos de acción (responsable + fecha límite) → Asuntos pendientes de confirmar.
Comparte el documento con los colegas que asistieron a la reunión.
```

### Notas importantes

- **Primero debes compartir la carpeta con el Bot**: El bot de Feishu no tiene directorio raíz de "mi espacio", si no compartes la carpeta destino con el Bot, las operaciones de creación/escritura fallarán
- **Wiki también necesita autorización**: Si quieres escribir en base de conocimientos, primero debes agregar el Bot al espacio Wiki correspondiente
- **Usar gestión de permisos con cuidado**: feishu-perm involucra cambios de permisos, se recomienda solo compartir carpetas específicas con el Bot, no dar permisos completos

## Enlaces relacionados

- [Guía completa de configuración de feishu-openclaw y soporte de comunidad](https://github.com/AlexAnys/openclaw-feishu) — Incluye comparación plugin oficial vs integrado, guía de migración, solución de problemas comunes
- [Guía de instalación del plugin oficial de Feishu](https://github.com/AlexAnys/openclaw-feishu/blob/main/docs/feishu-official-plugin.md)
- [Tutorial oficial ilustrado de Feishu](https://www.feishu.cn/content/article/7613711414611463386) — Producido por el equipo de Feishu
- [Documentación oficial de Feishu de OpenClaw](https://docs.openclaw.ai/zh-CN/channels/feishu)
- [Tencent Cloud - Tutorial completo: OpenClaw conectado a Feishu](https://cloud.tencent.com/developer/article/2626160)
- [Tencent Cloud - Dominar la suite completa de Feishu en 5 minutos](https://cloud.tencent.com/developer/article/2631667)
