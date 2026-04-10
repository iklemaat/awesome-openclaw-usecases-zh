# Plataforma de Servicio al Cliente IA Multicanal

> Incluye adaptación local: WeCom / Douyin / Xiaohongshu

Las pequeñas empresas necesitan manejar WhatsApp, mensajes directos de Instagram, correos electrónicos y reseñas de Google simultáneamente en múltiples aplicaciones. Los clientes esperan respuesta instantánea 24/7, pero contratar personal para cobertura 24/7 es costoso.

Este caso de uso integra todos los puntos de contacto con clientes en una bandeja de entrada unificada impulsada por AI, que responde en tu nombre.

## Descripción general de funciones

- **Bandeja de entrada unificada**: WhatsApp Business, mensajes directos de Instagram, Gmail y reseñas de Google en un solo lugar
- **Respuestas automáticas de AI**: Maneja automáticamente consultas frecuentes, solicitudes de citas y consultas generales
- **Escalado humano**: Escala problemas complejos o marca para revisión
- **Modo de prueba**: Demuestra el sistema a clientes sin afectar clientes reales
- **Contexto de negocios**: Entrenado en tus servicios, precios y políticas

## Caso comercial real

En Futurist Systems, desplegamos esta solución para empresas de servicio local (restaurantes, clínicas, salones de belleza). Un restaurante redujo el tiempo de respuesta de más de 4 horas a menos de 2 minutos, el 80% de las consultas se manejan automáticamente.

## Habilidades requeridas

- Integración con API de WhatsApp Business
- API Graph de Instagram (vía Meta Business)
- CLI `gog` para Gmail
- API de Perfil de Negocio de Google para gestión de reseñas
- Configurar lógica de enrutamiento de mensajes en AGENTS.md

## Cómo configurar

1. **Conectar canales vía configuración de OpenClaw**:
   - API de WhatsApp Business (vía 360dialog o API oficial)
   - Instagram (vía Meta Business Suite)
   - Gmail (vía OAuth de `gog`)
   - Token de API de Perfil de Negocio de Google

2. **Crear base de conocimientos de negocios**:
   - Servicios y precios
   - Horarios de atención y dirección
   - Respuestas a preguntas frecuentes
   - Condiciones de escalado (como quejas, solicitudes de reembolso)

3. **Configurar lógica de enrutamiento en AGENTS.md**:

```text
## 客户服务模式

当收到客户消息时：

1. 识别渠道（WhatsApp/Instagram/Email/Review）
2. 检查该客户是否启用了测试模式
3. 分类意图：
   - 常见问题 → 从知识库回复
   - 预约 → 检查可用时间，确认预订
   - 投诉 → 标记人工审核，确认收到
   - 评价 → 感谢反馈，回应关切

回复风格：
- 友好、专业、简洁
- 匹配客户的语言（ES/EN/UA）
- 绝不编造知识库中没有的信息
- 以商家名称结尾签名

测试模式：
- 回复前缀加 [TEST]
- 记录日志但不发送到真实渠道
```

4. **Configurar detección de latido (heartbeat) para monitoreo de respuesta**:

```text
## 心跳检测：客户服务检查

每 30 分钟：
- 检查超过 5 分钟未回复的消息
- 如果响应队列积压则发出警报
- 记录每日响应指标
```

## Ideas clave

- **Detección de lenguaje es importante**: Detecta y responde automáticamente en el idioma del cliente
- **Modo de prueba es esencial**: Los clientes necesitan ver el sistema en acción antes de lanzar
- **Reglas de escalado**: Define condiciones claras de escalado para evitar que la AI exceda autoridad
- **Plantillas de respuesta**: Pre-aprueba plantillas para temas sensibles (reembolsos, quejas)

## Enlaces relacionados

- [API de WhatsApp Business](https://developers.facebook.com/docs/whatsapp)
- [API de Mensajería de Instagram](https://developers.facebook.com/docs/instagram-api/guides/messaging)
- [API de Perfil de Negocio de Google](https://developers.google.com/my-business)

## Adaptación para usuarios de China

Los canales de alcance al cliente de empresas nacionales son completamente diferentes de los extranjeros. Los siguientes son esquemas de adaptación para canales nacionales principales.

### Tabla de comparación de canales

| Canal original | Reemplazo nacional | Viabilidad |
|---------|---------|--------|
| WhatsApp Business | Servicio al cliente de WeCom | Verificado viable |
| Mensajes directos de Instagram | Mensajes privados de cuenta empresarial de Douyin | Teóricamente viable, necesita calificación empresarial |
| Gmail | Correo empresarial | Directamente usable, sin adaptación necesaria |
| Reseñas de Google | Reseñas de Meituan/Dianping | Temporalmente no viable (API no abierta) |

### Canal núcleo P0: API de servicio al cliente de WeCom (verificado viable)

La plataforma abierta de WeCom proporciona una API completa de mensajes de servicio al cliente, soporta recepción de mensajes, respuestas activas y respuestas pasivas. Esta biblioteca de casos tiene un tutorial detallado de acceso a WeCom:

**Pre-requisitos**: Referir a [Asistente IA de WeCom](cn-wecom-ai-assistant.md) para completar el acceso básico.

**Puntos clave de configuración**:

1. Crear aplicación auto-construida en el backend de administración de WeCom, obtener Token y EncodingAESKey
2. Configurar URL de callback apuntando a la dirección del servicio OpenClaw
3. Soporta múltiples tipos de mensajes como texto, imagen, video, archivo
4. Nivel de riesgo: Bajo, usa API oficial

### Canal de extensión P1: Mensajes privados de cuenta empresarial de Douyin (teóricamente viable, necesita verificación)

La plataforma abierta de Douyin proporciona una interfaz de respuesta automática de mensajes privados de cuentas empresariales, se puede usar en escenarios de servicio al cliente.

**Limitaciones clave**:

- Solo soporta cuentas empresariales, cuentas personales no soportadas
- Las respuestas a mensajes privados no pueden enviar enlaces
- Solo se pueden enviar mensajes dentro de las 48 horas después de la última comunicación del cliente
- Los mensajes de bienvenida solo se pueden activar una vez por hora

### Canal de extensión P1: Plataforma de gestión de mensajes privados Xiaohongshu (teóricamente viable, necesita verificación)

Xiaohongshu oficialmente proporciona una plataforma de gestión de leads一站式 [Mensajes Privados](https://sxt.xiaohongshu.com/).

**Limitaciones clave**:

- Necesita cuenta empresarial certificada + certificación de plataforma JuGuang
- Los mensajes activos de extraños para cuentas Blue-V están limitados a 20 por día, máximo 3 por persona

### Temporalmente no viable: Respuesta a reseñas de Meituan/Dianping

La plataforma abierta de Meituan soporta consultar reseñas, pero la API de respuesta automática a reseñas no es pública. No cumple con el umbral de entrada de "ejecutado exitosamente", temporalmente no se incluye.

### Implementación de referencia multicanal

- **[ChatGPT-On-CS](https://github.com/cs-lazy-tools/ChatGPT-On-CS)** (open source AGPL-3.0): Ya soporta respuesta automática de servicio al cliente multi-plataforma WeChat/Pinduoduo/Qianniu/Bilibili/Douyin/Xiaohongshu/Zhihu, se puede usar como referencia técnica para integración de OpenClaw

### Recordatorio de seguridad

- Las credenciales de API de WeCom (Token, EncodingAESKey) se configuran vía variables de entorno, no hardcodear
- El contenido de respuesta de Douyin/Xiaohongshu necesita cumplir con las normas de comunidad de la plataforma, evitar activar control de viento
- Se recomienda configurar mecanismo de respaldo humano para respuestas automáticas, escalar a humano oportunamente para problemas complejos

> **Consejo de seguridad**: Las claves de API y Tokens de todas las plataformas son información sensible, por favor configura vía variables de entorno o archivo `.env`, asegura que `.env` se agregue a `.gitignore`.

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/multi-channel-customer-service.md)
