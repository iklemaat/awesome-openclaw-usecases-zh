# Asistente IA de DingTalk

DingTalk es la herramienta principal de muchas PYMES, pero las capacidades de IA integradas son limitadas y no cubren muchos escenarios. Quieres que la IA te ayude a organizar correos, buscar información, escribir documentos, pero las funciones integradas de DingTalk no pueden hacerlo, y no quieres que los miembros del equipo aprendan nuevas herramientas.

Este caso de uso despliega OpenClaw como un bot de DingTalk. Enviar mensajes en conversaciones de DingTalk puede activar tareas de IA, soporta modo Stream (no necesita IP pública), puede ejecutarse en computadora personal.

## Qué puede hacer

- **Asistente IA conversacional**: Conversar directamente con OpenClaw en chats privados o grupales de DingTalk
- **Modo Stream**: Conexión larga WebSocket, no necesita IP pública ni nombre de dominio
- **Soporte multimedia**: Soporta recepción y procesamiento de imágenes, voz, video, archivos (algunos tipos de salida dependen de la versión del plugin)
- **Salida de tarjetas IA en streaming**: Las respuestas se muestran en tiempo real como tarjetas de DingTalk
- **Respuestas Markdown**: Soporta mensajes con formato Markdown
- **Activación @ en grupos**: Responde solo cuando @al bot en grupos, no interfiere con la comunicación normal

## Habilidades requeridas

[openclaw-channel-dingtalk](https://github.com/soimy/openclaw-channel-dingtalk) (@soimy/dingtalk) — Plugin de canal DingTalk mantenido por la comunidad

## Cómo configurar

### Primer paso: Crear aplicación de DingTalk

Crear una aplicación empresarial interna en [Plataforma Abierta de DingTalk](https://open-dev.dingtalk.com), habilitar capacidades de bot.

### Segundo paso: Obtener credenciales

Registrar Client ID (AppKey) y Client Secret (AppSecret) en la página de información de la aplicación.

### Tercer paso: Configurar modo de recepción de mensajes

**Clave**: Seleccionar "Modo Stream" — así no se necesita IP pública ni nombre de dominio, la computadora personal puede ejecutar (similar al modo de conexión larga de Feishu).

### Cuarto paso: Instalar plugin y configurar

```bash
openclaw plugins install @soimy/dingtalk
```

Después de instalar, se debe agregar el plugin a la lista blanca de seguridad. Editar `~/.openclaw/openclaw.json`, agregar:

```json
{
  "plugins": {
    "enabled": true,
    "allow": ["@soimy/dingtalk"]
  }
}
```

A través de la guía interactiva de `openclaw onboard` o editar manualmente la configuración `channels` de `~/.openclaw/openclaw.json`, completar Client ID y Client Secret.

### Quinto paso: Iniciar y probar

```bash
openclaw gateway restart
```

Buscar tu bot en DingTalk, enviar mensaje para probar. Después de confirmar que funciona normalmente, configurar auto-inicio al encender:

```bash
openclaw gateway install
```

## Consejos prácticos

- **Modo Stream es el preferido**: Como Feishu, no necesita IP pública, computadora personal o NAS puede ejecutar
- **¿Elegir Feishu o DingTalk? Según el IM que use realmente tu equipo**: Cuál es la herramienta que abren cada día, conectar esa. Equipos que usan ambos pueden conectar simultáneamente
- **Estrategia de grupos**: Se recomienda configurar "@al bot para responder", evitar ser demasiado activo en grupos
- **Estrategia de seguridad**: Habilitar pairing/allowlist, limitar quién puede usar funciones avanzadas del bot

## Enlaces relacionados

- [openclaw-channel-dingtalk - GitHub](https://github.com/soimy/openclaw-channel-dingtalk)
- [Tencent Cloud - Tutorial completo: OpenClaw conectado a DingTalk](https://cloud.tencent.com/developer/article/2625121)
- [Tencent Cloud - Guía de acceso rápido](https://cloud.tencent.com/developer/article/2626553)
- [CSDN - Guía completa de conexión de DingTalk a OpenClaw](https://blog.csdn.net/weixin_42125125/article/details/158430832)
- [Alibaba Cloud - Solución de imagen preinstalada](https://help.aliyun.com/zh/simple-application-server/use-cases/quickly-deploy-and-use-openclaw)
