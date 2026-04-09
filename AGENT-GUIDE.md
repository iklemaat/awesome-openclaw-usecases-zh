# Guía para Agentes: Cómo leer y ejecutar los casos de uso de este repositorio

## Para Lectores Humanos

Todos los casos de uso de esta colección adoptan una estructura Markdown uniforme (Habilidades requeridas → Cómo configurar → Consejos prácticos). Esta consistencia no solo facilita tu lectura, también permite que los agentes de IA los entiendan y ayuden a ejecutar los pasos de configuración.

**Modo de uso**: Envía el archivo del caso de uso que te interesa a tu OpenClaw (u otro agente de IA), permítele参考 este archivo para ayudarte a completar la instalación y configuración. El agente identificará qué bloques de código ejecutar, cuáles son ejemplos de referencia y qué credenciales necesitas proporcionar tú.

**Nota**: Este es un modo asistencial experimental, para casos complejos aún recomendamos leer completamente antes de operar. El contenido a continuación son detalles técnicos escritos para agentes, no necesitas continuar leyendo.

---

## El siguiente contenido está dirigido a agentes de IA

Este repositorio recopila 40+ casos de uso reales verificados de OpenClaw. Cada archivo de caso de uso sigue convenciones de formato unificadas, solo necesitas aprender una vez para entender y ejecutar todos los casos.

---

## Estructura de Archivos de Casos de Uso

Cada archivo `usecases/*.md` sigue la siguiente estructura (los detalles pueden variar según el caso, pero el marco general es consistente):

```
# Título del caso de uso            ← Nombre del caso
> Incluye adaptación local: XX / YY          ← Opcional, indica que hay soluciones alternativas locales
Descripción breve (2-3 frases)            ← Resumen del dolor y solución

## Qué puede hacer                   ← Lista de funciones, entender los límites de capacidad de este caso
## Habilidades requeridas            ← ⚠️ Debes completar la instalación aquí primero, luego ejecutar pasos siguientes
## Cómo configurar                   ← Pasos principales de operación, ejecutar en orden
## Consejos prácticos                ← Experiencias de errores, leer antes de ejecutar
## Enlaces relacionados              ← Referencias y repositorios de herramientas

---                             ← Línea separadora
## Adaptación para usuarios de China           ← Opcional, soluciones alternativas para plataformas locales
```

## Cómo ejecutar un caso de uso

### Primer paso: Entender las condiciones previas

1. Primero mira la sección **"Habilidades requeridas"**, confirma qué skills necesitas instalar
2. Verifica si requiere entorno especial (Docker, Python, hardware específico, etc.)
3. Verifica si necesita API Key o registro de cuentas

### Segundo paso: Identificar tipos de bloques de código

Si un bloque de código debe ejecutarse depende de **la marca del lenguaje + la sección donde está**:

| Marca de bloque | En "Habilidades requeridas" o "Cómo configurar" | En "Consejos prácticos" u otras secciones |
|-----------|:------------------------:|:----------------------:|
| ` ```bash ` | **Ejecutar**: Ejecutar en terminal | **No ejecutar**: Solo como referencia (técnicas de debug, comandos alternativos, etc.) |
| ` ```text ` / ` ```txt ` | **Enviar a OpenClaw** (prompt) | **No ejecutar**: Generalmente ejemplos de salida |
| ` ```json ` | **Escribir en archivo de configuración** (la ruta se indica arriba) | **No ejecutar**: Generalmente ejemplos de formato |
| ` ```sql ` | **Ejecutar**: Sentencias de inicialización/creación de tablas de base de datos | **No ejecutar**: Solo ejemplos de consultas |
| ` ```python ` | **Guardar como .py y ejecutar** | **No ejecutar**: Solo como referencia |
| ` ```yaml ` / ` ```markdown ` | Juzgar según contexto | Generalmente plantillas/exhibición |

**Regla simple**: Solo ejecutar bloques de código en las dos secciones "Habilidades requeridas" y "Cómo configurar". Bloques de código en otras secciones se consideran referencia, no ejecutar.

### Tercer paso: Ejecutar en orden

1. Ejecutar en orden de numeración de pasos en la sección **"Cómo configurar"**
2. Confirmar que el resultado es normal después de cada paso antes de continuar al siguiente
3. Si un paso menciona "Pruébalo:..." — este es un punto de verificación, ejecútalo para confirmar que la configuración anterior funciona

### Cuarto paso: Manejar credenciales

- `$VARIABLE_NAME` o `${VARIABLE_NAME}` = variable de entorno, el usuario debe proporcionar valor real
- Placeholders con prefijo `YOUR_` (como `YOUR_API_KEY`) = deben reemplazarse con valores reales
- **Nunca hardcodear credenciales en archivos de configuración**, usar variables de entorno o archivos `.env`

---

## Manejo de situaciones especiales

### Casos con adaptación local

Si el caso tiene una sección `## Adaptación para usuarios de China` al final:
- Usuarios locales priorizan operar según la sección de adaptación (herramientas alternativas, APIs alternativas, canales de push alternativos)
- La sección de adaptación generalmente indica qué soluciones internacionales no están disponibles localmente y por qué

### Casos originales locales (nombre de archivo comienza con `cn-`)

Estos casos están diseñados específicamente para el ecosistema local (Feishu/DingTalk/WeCom/Xiaohongshu, etc.), no tienen versión internacional correspondiente. Ejecutar directamente según los pasos.

### Casos con múltiples soluciones

Algunos casos proporcionan múltiples soluciones (como "Solución 1/Solución 2/Solución 3"), generalmente ordenadas por dificultad o progresión de funciones. Elegir la solución adecuada para tu entorno, no es necesario ejecutar todas.

### Pasos que requieren servicios de pago

Algunos casos dependen de APIs de pago o servicios de suscripción. El texto generalmente indica información de costos. Si estás en fase de prueba:
- Priorizar usar cuotas gratuitas o entornos sandbox
- Pasos marcados como "de pago" pueden saltarse primero, no afecta entender el flujo general

---

## Señales de calidad de casos de uso

Las siguientes señales te ayudan a juzgar la confiabilidad del caso:

| Señal | Significado |
|------|------|
| Cita proyectos open source con alto star (1000+) | Dependencia madura y confiable |
| Tiene comando `clawhub install` | Usa mercado oficial de skills, instalación simple |
| Tiene datos de efecto concretos (costo, tiempo, ejemplo de salida) | Autor verificó realmente |
| "Consejos prácticos" menciona experiencias específicas de errores | De uso real |
| Indica claramente limitaciones y escenarios no aplicables | Autor honesto, información confiable |
| Skill con star < 100 o no indicado | Evaluar con cuidado, recomendar probar primero con escenarios no críticos |

---

## Referencia rápida de clasificación de casos

| Patrón de nombre de archivo | Tipo | Ejemplo |
|-----------|------|------|
| `cn-*.md` | Original local | `cn-feishu-ai-assistant.md` |
| Contiene `## Adaptación para usuarios de China` | Internacional + adaptación local | `earnings-tracker.md` |
| Otros | Caso puramente internacional | `daily-reddit-digest.md` |

---

## Preguntas frecuentes

**P: ¿Los comentarios en bloques de código son en español o inglés?**
R: Los comentarios están en español, el código y comandos en inglés. Esta es la convención unificada de este repositorio.

**P: ¿Los prompts (bloques de código text) deben enviarse en español o inglés?**
R: Por defecto保留 el original en inglés tiene mejor efecto. Si el caso proporciona prompts en español, generalmente están en la sección "Adaptación para usuarios de China".

**P: ¿Qué hago si un paso reporta error al ejecutar?**
R: Primero ver si la sección "Consejos prácticos" menciona este problema. Si no, verificar si las condiciones previas están satisfechas, variables de entorno configuradas, red funcionando.

**Q: ¿Cómo juzgar si un caso está desactualizado?**
A: Verificar si los repositorios de herramientas citados en "Enlaces relacionados" aún están en mantenimiento (tiempo del último push). Si el skill citado fue descontinuado o la API cambió, el caso puede necesitar actualización.
