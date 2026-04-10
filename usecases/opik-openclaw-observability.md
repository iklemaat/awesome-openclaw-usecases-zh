# Seguimiento de Observabilidad de OpenClaw + Opik

Después de que OpenClaw comienza a albergar negocios reales, los problemas generalmente surgen de lo "invisible": una conversación desencadena llamadas de LLM, llamadas de herramientas, colaboración de sub-agentes, pero la información de resolución de problemas está dispersa en diferentes registros, haciendo que la localización sea costosa.

Este caso de uso demuestra cómo conectar el proceso de ejecución de OpenClaw a Opik vía `@opik/opik-openclaw`, logrando seguimiento unificado, resolución de errores y monitoreo de costos.

## Dolor

- **Pipeline invisible**: Es difícil correlacionar una solicitud de usuario con todos los pasos de ejecución subsiguientes
- **Resolución de fallos lenta**: La resolución requiere cambiar entre registros de gateway, registros de modelos y registros de herramientas
- **Costos poco transparentes**: Capacidad insuficiente para observar tokens y costos por tarea/flujo de trabajo

## Descripción de funciones

- Exportar eventos de OpenClaw como trace/span en Opik
- Cubre los siguientes datos clave:
  - Span de solicitud y respuesta de LLM
  - Span de llamadas de herramientas (parámetros de entrada/resultados/errores)
  - Span de ciclo de vida de sub-agentes
  - Metadatos al final de la ejecución
  - Metadatos de uso y costos

## Prompt (se puede copiar directamente)

```text
请做一次网关健康检查，发送一条测试消息，并汇总今天 Opik 里所有失败的工具调用及错误原因。
```

```text
请统计最近 24 小时 Opik 追踪里 token 和成本最高的工作流，并给出每个工作流 1 条优化建议。
```

## Habilidades requeridas

- Habilidad básica de uso de CLI de OpenClaw
- Sin skills adicionales de OpenClaw necesarios

## Método de configuración

1. Instalar plugin:

```bash
openclaw plugins install @opik/opik-openclaw
```

2. Ejecutar asistente de configuración:

```bash
openclaw opik configure
```

3. Verificar configuración efectiva:

```bash
openclaw opik status
```

4. Enviar solicitud de prueba y generar seguimiento:

```bash
openclaw gateway run
openclaw message send "hello from openclaw"
```

5. Confirmar que trace y span se almacenan normalmente en Opik.

## Enlaces relacionados

- [Repositorio de opik-openclaw](https://github.com/comet-ml/opik-openclaw)
- [Documentación de Opik](https://www.comet.com/docs/opik/)
- [Documentación de plugins de OpenClaw](https://docs.openclaw.ai/plugin)
