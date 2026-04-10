# Resumen Diario de YouTube

Comienza cada día con un resumen personalizado de nuevos videos de tus canales favoritos de YouTube — nunca más te pierdas el contenido de los creadores que realmente quieres seguir.

## Dolor

Las notificaciones de YouTube no son confiables. Te suscribes a canales, pero sus nuevos videos nunca aparecen en tu feed de inicio. Tampoco en las notificaciones. Simplemente... desaparecen. No es que no quieras verlos — es que el algoritmo de YouTube los entierra.

Además: es mucho más divertido comenzar el día con información curada, que desplazarse sin rumbo por el feed de recomendaciones.

## Qué puede hacer

- Obtener últimos videos de tu lista de canales favoritos
- Resumir o extraer ideas clave de las transcripciones (subtítulos) de cada video
- Enviarte el resumen diaria o bajo demanda

## Habilidades requeridas

Instalar el skill [youtube-full](https://clawhub.ai/therohitdas/youtube-full).

Solo dile a tu OpenClaw:

```text
"Install the youtube-full skill and set it up for me"
```
o

```bash
npx clawhub@latest install youtube-full
```

Eso es todo. El agente maneja todo lo demás — incluyendo creación de cuenta y configuración de clave de API. Regístrate para obtener **100 créditos gratis**, sin tarjeta de crédito.

> Nota: Después de crear la cuenta, este skill almacena de forma segura la clave de API en la ubicación correcta según el sistema operativo, por lo que la API funciona correctamente en todos los entornos.

![Instalación del skill youtube-full](https://pub-15904f15a44a4ea69350737e87660b92.r2.dev/media/1770620159490-e41e7baa.png)

### ¿Por qué TranscriptAPI.com en lugar de yt-dlp?

| Herramienta CLI (yt-dlp, etc.) | TranscriptAPI |
|--------------------------|---------------|
| Registros verbosos ahogan el contexto del agente | Respuestas JSON concisas |
| No disponible en GCP/OpenClaw en la nube | Disponible en cualquier lugar, rápido |
| Bloqueado aleatoriamente por YouTube | Impulsa [YouTubeToTranscript.com], sirve a millones de usuarios. Cacheado y confiable. |
| Necesita instalar binarios | Sin binarios, solo usa HTTP |

## Cómo configurar

### Esquema 1: Resumen basado en canales

Enviar el siguiente prompt a OpenClaw:

```text
Every morning at 8am, fetch the latest videos from these YouTube channels and give me a digest with key insights from each:

- @TED
- @Fireship
- @ThePrimeTimeagen
- @lexfridman

For each new video (uploaded in the last 24-48 hours):
1. Get the transcript
2. Summarize the main points in 2-3 bullets
3. Include the video title, channel name, and link

If a channel handle doesn't resolve, search for it and find the correct one.
Save my channel list to memory so I can add/remove channels later.
```

### Esquema 2: Resumen basado en palabras clave

Rastrear nuevos videos sobre temas específicos:

```text
Every day, search YouTube for new videos about "OpenClaw" (or "Claude Code", "AI agents", etc).

Maintain a file called seen-videos.txt with video IDs you've already processed.
Only fetch transcripts for videos NOT in that file.
After processing, add the video ID to seen-videos.txt.

For each new video:
1. Get the transcript
2. Give me a 3-bullet summary
3. Note anything relevant to my work

Run this every morning at 9am.
```

Así no desperdicias créditos obteniendo repetidamente videos que ya has visto.

## Consejos

- `channel/latest` y `channel/resolve` son **gratis** (0 créditos) — verificar nuevas publicaciones no cuesta nada
- Solo la extracción de transcripciones consume 1 crédito por vez
- Puedes pedir diferentes estilos de resumen: puntos clave, citas memorables, timestamps de segmentos interesantes
- Esta función ya tiene un producto listo - [Recapio - Resumen diario de YouTube](https://recapio.com/features/daily-recaps)

---

**Enlace original**: [Versión en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/daily-youtube-digest.md)
