# Automatización de Contenido de Xiaohongshu

Los operadores de medios_selfies siguen tendencias diariamente en Xiaohongshu → escribir copy → crear portadas → programar publicación. Cada nota desde la concepción hasta la publicación toma al menos una hora, si publicas múltiples cuentas diariamente, el tiempo se duplica.

Este caso de uso usa Skills de OpenClaw desarrollados por la comunidad para llamar scripts de automatización Python, auxiliar en el flujo completo desde selección de temas, redacción de copy, generación de portadas hasta publicación.

## Qué puede hacer

- **Detección de tendencias**: Analizar automáticamente temas populares actuales en Xiaohongshu
- **Generación de copy**: IA genera notas gráficas y de texto según el estilo de escritura único de Xiaohongshu (incluye título, cuerpo, etiquetas)
- **Generación de portadas**: Llamar modelos de IA de imagen para generar portadas profesionales
- **Publicación automática**: Programar publicación de notas después de iniciar sesión
- **Seguimiento de datos**: Ver datos de vistas, likes, favoritos de las notas

## Habilidades requeridas

[XiaohongshuSkills](https://github.com/white0dew/XiaohongshuSkills) — Habilidades de automatización de Xiaohongshu desarrolladas por la comunidad. **Actualmente solo probado en Windows**, usuarios de macOS/Linux necesitan verificar compatibilidad por sí mismos.

Se necesita Python 3.10+ y navegador Chrome.

## Cómo configurar

1. Clonar proyecto e instalar dependencias:
```bash
git clone https://github.com/white0dew/XiaohongshuSkills.git
cd XiaohongshuSkills
pip install -r requirements.txt
```

2. Iniciar sesión en la cuenta (primera vez necesita escanear código QR):
```bash
python scripts/cdp_publish.py login
```

3. Colocar el directorio del proyecto en el directorio skills de OpenClaw (como `~/.openclaw/workspace/skills/`), para que OpenClaw pueda reconocer esta Skill.

4. Después de configurar, usar lenguaje natural para publicar contenido en la conversación de OpenClaw:
```text
Genera una nota gráfica y de texto de recomendación de belleza, publica mañana a las 10 AM.

Ayúdame a escribir una nota de Xiaohongshu sobre "técnicas de organización del hogar", con un título atractivo y 5 etiquetas de tema relacionadas.

Analiza las notas populares recientes relacionadas con "comidas de gimnasio", encuentra puntos en común de alto like.
```

## Consejos prácticos

- **Calidad de contenido primero**: Se recomienda revisar manualmente el contenido generado por IA antes de publicar, asegurar que la calidad del contenido y la tonicidad de la marca sean consistentes
- **Controlar frecuencia de publicación**: La publicación automática demasiado frecuente puede activar el control de viento de la plataforma, se recomienda no más de 3-5 notas diarias
- **Gestión de múltiples cuentas**: Esta skill soporta gestión de Cookies aislada para múltiples cuentas, puede gestionar múltiples cuentas operadas simultáneamente
- **Iteración impulsada por datos**: Usar la función de seguimiento de datos para analizar qué tipo de contenido tiene mejor rendimiento, dejar que OpenClaw recuerde tus preferencias y optimice continuamente la selección de temas

> **⚠️ Recordatorio de riesgo**: Xiaohongshu tiene estrategias estrictas de control de viento para operaciones automatizadas, incluyendo pero no limitado a detección de huellas digitales de Chrome, límites de frecuencia de publicación, análisis de comportamiento de cuentas, etc. Las operaciones automatizadas pueden llevar a limitaciones de flujo o bloqueo de cuentas. **Se recomienda fuertemente usar cuentas de prueba para verificar antes de usar en cuentas formales**, principalmente para auxiliar creación, evitar llenado masivo.

## Enlaces relacionados

- [XiaohongshuSkills - GitHub](https://github.com/white0dew/XiaohongshuSkills)
- [CSDN - Caso práctico: Usar OpenClaw para publicar automáticamente notas de Xiaohongshu](https://blog.csdn.net/weixin_49598732/article/details/158286134)
- [Tencent Cloud - Dejar Xiaohongshu a OpenClaw](https://cloud.tencent.com/developer/article/2629870)
