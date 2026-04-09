# Asistente IA de WeCom (企业微信)

WeChat es la herramienta de comunicación más usada en China, pero las capacidades de IA no se pueden usar directamente en el ecosistema de WeChat — tienes que cambiar a otra App para usar IA, y luego mover manualmente los resultados de vuelta a WeChat.

Este caso de uso despliega OpenClaw como una aplicación de WeCom (企业微信). Enviar mensajes en WeCom puede activar tareas de IA, y a través de la función "plugin de WeChat" de WeCom, los usuarios de WeChat personal pueden escanear el código QR para asociarse y también conversar directamente con la IA en WeChat.

## Qué puede hacer

- **IA conversacional en WeCom**: Conversar directamente con OpenClaw en chats privados o grupales de WeCom
- **WeChat personal también puede usar**: A través del "plugin de WeChat" de WeCom, después de asociar, usuarios de WeChat personal también pueden conversar con IA
- **Salida en streaming**: Las respuestas de IA se muestran carácter por carácter en tiempo real
- **Asistente IA para grupos**: Soporta activación por @, lista blanca de comandos y otras estrategias
- **Soporte multimedia**: Soporta envío y procesamiento de imágenes y archivos

## Dos esquemas de integración

| Esquema | Plugin | Características |
|------|------|------|
| Suite openclaw-china | `@openclaw-china/wecom-app` | Colección de plugins de IM de China mantenidos por la comunidad, configuración todo en uno |
| Plugin sunnoy | `@sunnoy/wecom` | Funciones más ricas, soporta gestión dinámica de Agentes, integración de grupos, lista blanca de comandos |

Ambos esquemas están verificados por la comunidad, elige según tu necesidad: para conversación básica elige el primero, para funciones avanzadas como gestión de grupos elige el segundo.

## Habilidades requeridas

- Versión OpenClaw 2026.2.9+
- Permisos de administrador de WeCom (para crear aplicación auto-construida)
- IP pública o herramienta de penetración de red interna (para recibir callbacks)

## Cómo configurar

### Primer paso: Crear aplicación de WeCom

En [Backend de Administración de WeCom](https://work.weixin.qq.com) → Gestión de Aplicaciones → Crear Aplicación. Registrar Corp ID, Corp Secret, Agent ID.

### Segundo paso: Configurar URL de callback

Configurar la URL de callback de "recepción de mensajes" en la configuración de la aplicación, el formato general es:

```
http://<tu IP pública>:18789/wecom-app
```

Si no tienes IP pública, puedes usar herramientas de penetración de red interna (como frp, ngrok).

### Tercer paso: Instalar plugin y configurar

**Esquema A: Suite openclaw-china**

```bash
openclaw plugins install @openclaw-china/wecom-app
```

Configurar en `openclaw.json`:

```json
{
  "channels": {
    "wecom-app": {
      "enabled": true,
      "webhookPath": "/wecom-app",
      "token": "<Token de WeCom>",
      "encodingAESKey": "<Clave AES de WeCom>",
      "corpId": "<Corp ID>",
      "corpSecret": "<Corp Secret>",
      "agentId": "<Agent ID>"
    }
  }
}
```

> **Recordatorio de seguridad**: Por favor no escribas credenciales reales directamente en el archivo de configuración y las envíes al control de versiones. Se recomienda usar variables de entorno: guardar información sensible como Token, Clave AES, etc. en un archivo `.env`, y asegurar que `.env` se agregue a `.gitignore`.

**Esquema B: Plugin sunnoy**

```bash
openclaw plugins install @sunnoy/wecom
```

Configurar channels según la [documentación del plugin](https://github.com/sunnoy/openclaw-plugin-wecom). Este plugin soporta modo Bot y modo App, los parámetros de configuración específicos por favor consulta el README del repositorio.

> Después de instalar plugins de comunidad de terceros, se recomienda configurar lista blanca en `plugins.allow` (referir la descripción en el caso de uso de DingTalk).

### Cuarto paso: Iniciar y probar

```bash
openclaw gateway restart
```

Encontrar tu aplicación en WeCom, enviar un mensaje para probar.

### Quinto paso (opcional): Asociar WeChat personal

En el backend de administración de WeCom → Plugin de WeChat → Invitar miembros a asociar WeChat personal. Después de asociar, los miembros pueden conversar directamente con la IA en WeChat personal.

## Consejos prácticos

- **Recomendamos fuertemente la ruta de WeCom**: Usar API oficial, cero riesgo de bloqueo, estable y confiable. No se recomiendan esquemas de protocolos de terceros de WeChat personal (enfrentan bloqueo en cualquier momento)
- **Penetración de red interna**: A diferencia del modo de conexión larga de Feishu, WeCom necesita una URL de callback accesible públicamente. Si despliegas en casa, necesitas configurar penetración de red interna
- **Estrategia de seguridad**: Se recomienda habilitar pairing/allowlist, grupos configurar como activación por @, evitar que cualquiera pueda usar
- **El plugin de WeChat es una función杀手**: Te permite no necesitar que todos instalen WeCom, después de asociar con WeChat personal se puede usar directamente

## Enlaces relacionados

- [Plugin de WeCom de openclaw-china](https://github.com/BytePioneer-AI/openclaw-china)
- [Plugin de WeCom de sunnoy (funciones avanzadas)](https://github.com/sunnoy/openclaw-plugin-wecom)
- [Tutorial oficial de Alibaba Cloud - Conexión de WeCom a OpenClaw](https://help.aliyun.com/zh/simple-application-server/use-cases/openclaw-enterprise-wechat-integration)
- [Tutorial de integración de WeCom de Tencent Cloud](https://cloud.tencent.com/developer/article/2625147)
- [Comunidad de Desarrolladores de Alibaba Cloud - Tutorial completo 2026](https://developer.aliyun.com/article/1711514)
