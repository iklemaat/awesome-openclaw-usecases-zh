# Resumen Diario de Reddit

Ejecuta un resumen diario cada día, te proporciona las mejores publicaciones de tus subreddits favoritos.

Escenarios de uso:

• Navegar por subreddits (publicaciones populares/recientes/fijadas)
• Buscar publicaciones por tema
• Extraer hilos de comentarios para obtener contexto
• Establecer una lista de publicaciones para revisar/responder manualmente después

> Esto es solo lectura. No soporta publicar, votar o comentar.

## Habilidades requeridas

Skill [reddit-readonly](https://clawhub.ai/buksan1950/reddit-readonly). No requiere autenticación.

## Cómo configurar

Después de instalar la skill, envía el siguiente prompt a tu OpenClaw:

El siguiente prompt se puede usar directamente:

```text
Quiero que me des las publicaciones con mejor rendimiento de los siguientes subreddits.
<pega la lista aquí>
Crea una memoria separada para los procesos de reddit, sobre el tipo de publicaciones que me gusta ver y cada día pregúntame si te gustó la lista que proporcionaste. Guarda mi preferencia como reglas en la memoria para usar para una mejor curación del resumen. (por ejemplo, no incluir memes.)
Cada día a las 5 PM, ejecuta este proceso y dame el resumen.
```

### Ejemplo de lista de subreddits

Aquí hay un ejemplo de subreddits que podrías querer seguir:

```text
- r/technology
- r/artificial
- r/programming
- r/startups
- r/entrepreneurship
- r/investing
```

### Personalización del resumen

Puedes personalizar el resumen enviando mensajes a tu agente:

```text
Agrega r/science a mi resumen diario.
Deja de incluir publicaciones de r/funny.
Solo muestra publicaciones con más de 100 upvotes.
Incluye los 3 mejores comentarios de cada publicación.
```

### Cambio de horario

Para ajustar la hora del resumen diario:

```text
Cambia el resumen diario a las 8 AM en lugar de las 5 PM.
Configura la zona horaria a Europe/Madrid.
```

## Consejos prácticos

- **Empieza con pocos subreddits**: Comienza con 3-5 subreddits principales, luego agrega más según sea necesario para evitar sobrecarga de información
- **Filtra por calidad**: Pide al agente que excluya memes, contenido de baja calidad o temas que no te interesan
- **Guarda preferencias**: El agente recordará tus preferencias con el tiempo y mejorará la curación del resumen
- **Revisa los comentarios**: Las mejores perspectivas a menudo están en los comentarios, no solo en las publicaciones
- **Ajusta con el tiempo**: Tu lista de subreddits ideal evolucionará, está bien hacer cambios

## Adaptación para otras plataformas

Este caso de uso se puede adaptar fácilmente para otras plataformas de redes sociales:

- **Hacker News**: Para noticias de tecnología y programación
- **Product Hunt**: Para descubrir nuevos productos
- **Twitter/X**: Para tendencias en tiempo real
- **Lobsters**: Para discusiones técnicas de mayor calidad

Cada plataforma requiere su propia skill, pero la estructura del resumen es similar.

## Enlaces relacionados

- [Skill reddit-readonly en ClawHub](https://clawhub.ai/buksan1950/reddit-readonly)
- [Documentación oficial de Reddit API](https://www.reddit.com/dev/api/)
- [Versión original en inglés](https://github.com/AlexAnys/awesome-openclaw-usecases/blob/main/usecases/daily-reddit-digest.md)
