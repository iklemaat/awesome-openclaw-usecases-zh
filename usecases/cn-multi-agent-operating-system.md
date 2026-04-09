# Sistema Operativo de Colaboración Multi-Agente (OpenCrew)

OpenClaw después de usarlo por un tiempo encuentra un conjunto de problemas comunes: un solo agente maneja múltiples áreas llevando a expansión de contexto, falta de interfaz de gestión unificada cuando múltiples proyectos se ejecutan en paralelo, agentes modificando configuraciones por sí mismos causando fallos, y experiencia y conocimiento acumulados en conversaciones desaparecen con el historial de chat.

OpenCrew convierte OpenClaw de "un agente que hace de todo" a "un equipo de IA con divisiones de trabajo", a través de Slack, Discord, Feishu para gestión y programación multi-hilo (usuarios en China continental necesitan VPN para acceder a Slack, Discord).

## Qué puede hacer

- **División de trabajo multi-agente**: Cada agente tiene responsabilidades claras — Estratega Jefe (CoS), Director Técnico (CTO), Ejecutor (Builder), etc.
- **Slack como interfaz de gestión**: Cada canal corresponde a la "estación de trabajo" de un agente, cada Thread es una tarea independiente. Tus mensajes no leídos son la cola de pendientes
- **Alineación de intenciones**: Estratega confirma tu intención real antes de ejecutar, previene que el agente "resuelva eficientemente el problema incorrecto"
- **Avance activo**: Los agentes pueden avanzar autónomamente según nivel de autorización cuando no estás en línea
- **Sedimentación de conocimiento**: Cada tarea completada extrae experiencia reutilizable estructurada, no desaparece con el historial de chat
- **Auditoría de cambios**: Agente Ops audita todas las auto-modificaciones, previene deriva de configuración y fallos

## Arquitectura central

```
Tú (Tomador de decisiones)
 └── Estratega CoS (Alineación de intenciones, avance asíncrono)
       ├── CTO (Decisiones de arquitectura técnica)
       ├── Builder (Ejecución y entrega)
       ├── CIO (Experto de dominio, reemplazable)
       └── Ops (Auditoría de cambios, anti-deriva)
            └── Oficial de Conocimiento (Extracción y sedimentación de conocimiento)
```

Configuración mínima usable solo necesita 3 agentes: CoS + CTO + Builder.

## Mecanismos clave

### Clasificación de decisión autónoma

Decisiones se clasifican por **reversibilidad** en lugar de complejidad, decide cuándo el agente puede actuar por sí mismo, cuándo necesita consultar:

| Nivel | Significado | Comportamiento |
|:----:|------|------|
| L0 | Solo sugerencias | Solo proporciona sugerencias, no ejecuta |
| L1 | Operaciones reversibles | Ejecuta autónomamente, errores se pueden revertir |
| L2 | Tiene impacto pero recuperable | Presenta informe de trabajo después de ejecutar |
| L3 | Operaciones irreversibles | Debe esperar tu aprobación |

### Compresión de conocimiento de tres capas

| Capa | Contenido | Ratio de compresión |
|:----:|------|:------:|
| L0 | Historial de conversaciones original | 1x |
| L1 | Informe de trabajo estructurado (10-15 líneas) | 25x |
| L2 | Conocimiento abstracto reutilizable | 100x+ |

### Comunicación entre agentes (protocolo A2A)

Colaboración entre agentes tiene control estricto de seguridad:
- Solo CoS/CTO/Ops tienen derecho a iniciar comunicación cross-agente
- Cada interacción limitada a 4-5 rondas, previene bucles infinitos
- Toda comunicación visible en Slack, garantiza transparencia

## Habilidades requeridas

- Versión OpenClaw 2026.2+
- Espacio de trabajo de Slack (versión gratis funciona, China continental necesita VPN para acceder)
- [Marco OpenCrew](https://github.com/AlexAnys/opencrew)

## Cómo configurar

Guía completa de despliegue por favor consulta [Documentación OpenCrew](https://github.com/AlexAnys/opencrew), pasos principales:

1. **Crear App de Slack** (Modo Socket, no necesita red pública):
   - Obtener App Token (`xapp-...`) y Bot Token (`xoxb-...`)
   - Configurar suscripción de eventos y permisos

2. **Crear canales de Slack**:
   - Mínimo 3: `#hq` (CoS), `#cto` (CTO), `#build` (Builder)
   - Opcional: `#know` (Conocimiento), `#ops` (Auditoría)

3. **Desplegar configuración de agentes**:
   - Cada agente tiene archivo de configuración estandarizado (definición de rol, flujo de trabajo, preferencias de usuario, memoria, etc.)
   - Protocolos compartidos a través de symlink aseguran que todos los agentes sigan especificaciones unificadas

4. **Verificar ejecución**: Enviar mensaje a `#hq`, confirmar que CoS responde normalmente

## Escenarios aplicables

- **Múltiples proyectos en paralelo**: Desarrollar producto, investigación de mercado, operación de contenido simultáneamente, cada proyecto en Thread independiente
- **Mejora de colaboración en equipo**: Una persona + equipo de IA = una persona completa carga de trabajo de pequeño equipo
- **Gestión de proyectos a largo plazo**: Experiencia no desaparece con conversaciones, conocimiento se acumula continuamente
- **Estabilidad de configuración**: No más preocupación de que agentes modifiquen configuraciones por sí mismos causando fallos

## Enlaces relacionados

- [OpenCrew - GitHub](https://github.com/AlexAnys/opencrew)
- [Guía de despliegue OpenCrew](https://github.com/AlexAnys/opencrew/blob/main/DEPLOY.md)
- [Documentación de conceptos centrales OpenCrew](https://github.com/AlexAnys/opencrew/blob/main/docs/CONCEPTS.md)

## Notas para usuarios de China

- **Requisito de red**: Slack no se puede acceder directamente en China continental, necesita usar VPN. Al desplegar en servidor en la nube asegurar que servidor pueda acceder a API de Slack
- **Exploración de esquema alternativo**: Si el equipo usa principalmente Feishu, prestar atención al progreso de soporte de OpenCrew para Feishu. Esquema de integración de Feishu referencia [guía de configuración de openclaw-feishu](https://github.com/AlexAnys/openclaw-feishu)
- **Configuración mínima**: Al intentar por primera vez se recomienda comenzar con 3 agentes (CoS + CTO + Builder), después de ejecutar expandir
