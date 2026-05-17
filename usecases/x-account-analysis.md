# Análisis de Cuenta de X

Hay muchos sitios web especializados que te proporcionan análisis cualitativo de tu cuenta de X. Aunque X ya proporciona un panel de **análisis**, se enfoca más en mostrar tu rendimiento de datos.

El análisis cualitativo se enfoca en la calidad de tus publicaciones, no en estadísticas de rendimiento. Algunos insights que puedes obtener de este tipo de análisis incluyen:
- ¿Qué patrones hacen que mis publicaciones se vuelvan virales?
- ¿Sobre qué temas hablo para obtener más interacción?
- ¿Por qué algunas de mis publicaciones obtienen 1000+ likes, pero a veces las publicaciones tienen menos de 5 likes? ¿Qué hice mal?

Hay muchos sitios web y aplicaciones que ofrecen análisis de X, pero se enfocan en estadísticas. Puede que solo haya 1-2 sitios web que te permitan conversar con AI para entender tu rendimiento.

Pero ahora puedes usar OpenClaw para hacer este análisis por ti, sin necesidad de pagar suscripciones mensuales de $10-50 en estos sitios web.

## Habilidades requeridas

Skill Bird. `clawhub install bird` (ya preinstalado)

Ruta opcional con API: [TweetClaw](https://github.com/Xquik-dev/tweetclaw), plugin OpenClaw publicado como [`@xquik/tweetclaw`](https://www.npmjs.com/package/@xquik/tweetclaw). Úsalo cuando prefieras trabajar con una API key local en lugar de copiar cookies del navegador, o cuando necesites buscar tweets, buscar respuestas, exportar seguidores, consultar usuarios, monitorear keywords, recibir webhooks o preparar borradores de tweets y respuestas con revisión humana.

```bash
openclaw plugins install @xquik/tweetclaw
```

Referencia de instalación: [TweetClaw en ClawHub](https://clawhub.ai/plugins/@xquik/tweetclaw).

## Cómo configurar

Este es el flujo:
1. Asegurar que el skill Bird funcione correctamente.
2. Por seguridad y aislamiento, mejor crear una nueva cuenta para tu ClawdBot.
3. Autenticar con tu cuenta de X. Iniciar sesión en x.com en Chrome/Brave, y proporcionar información correcta de cookie (credenciales web) (`auth-token`, `ct0`), para que OpenClaw pueda acceder a tu cuenta.
4. Dejar que OpenClaw vea tu cuenta real, obtener las N tweets recientes, luego puedes preguntar libremente. O también puedes pedirle que escriba scripts específicos para ti.

Flujo opcional con TweetClaw:
1. Instalar el plugin con `openclaw plugins install @xquik/tweetclaw`.
2. Guardar `XQUIK_API_KEY` solo en tu entorno local de OpenClaw. No lo escribas en prompts, archivos compartidos ni repositorios.
3. Usar `explore` para revisar las herramientas disponibles y confirmar que `tweetclaw` está permitido.
4. Pedir primero análisis de solo lectura: buscar tweets, buscar respuestas, exportar seguidores, consultar usuarios o monitorear keywords.
5. Para publicar tweets, responder, enviar DMs, seguir, retuitear, dar like o subir media, exige que el agente muestre cuenta, texto final, objetivo y media antes de aprobar.

## Consejos prácticos

- **Comienza con un subconjunto**: Primero analiza las últimas 50-100 publicaciones, identifica patrones antes de analizar todo el historial
- **Enfócate en métricas accionables**: No solo mires likes — analiza tiempo de publicación, longitud del tweet, uso de hashtags, menciones, etc.
- **Compara períodos**: Compara tu rendimiento antes/después de cambios en tu estrategia de contenido
- **Identifica tu voz única**: ¿Qué te hace diferente de otros en tu nicho? El análisis puede ayudar a identificar esto
- **Respeta límites de plataforma**: evita acciones masivas, scraping agresivo o automatización visible sin aprobación explícita
- **Separa lectura y escritura**: usa TweetClaw primero para investigación, monitoreo y borradores; publica solo después de revisión humana

## Preguntas que puedes hacer

Después de que OpenClaw analiza tu cuenta, puedes hacer preguntas como:

```text
¿Cuáles son mis 5 tweets con mejor rendimiento del último mes? ¿Qué tienen en común?

¿En qué días y horas obtengo más interacción?

¿Qué temas debería evitar porque no resuenan con mi audiencia?

Basado en mis datos, ¿qué debería publicar más?

Usando TweetClaw, busca tweets y respuestas de los últimos 30 días sobre mi producto. Agrupa quejas, preguntas y elogios, y cita los enlaces relevantes.

Exporta mis seguidores recientes con TweetClaw, resume los perfiles más relevantes para investigación de audiencia y no contactes a nadie sin aprobación.

Prepara 3 respuestas a tweets recientes, pero no publiques. Muestra texto final, tweet objetivo y riesgo de plataforma para revisión.
```

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/x-account-analysis.md)
