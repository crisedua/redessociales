# 📦 Cómo instalar las Skills de Redes Sociales en Claude Desktop

Las 17 skills se instalan juntas como **un solo plugin**. Solo necesitas un archivo:
**`redes-sociales-plugin.zip`**.

---

## Instalación

1. Descarga el archivo **`redes-sociales-plugin.zip`** y guárdalo en tu computadora.
2. Abre **Claude Desktop** → **Configuración (Settings)** → **Plugins**.
3. Ve a la pestaña **Local uploads** y haz clic en **+** → **Upload local plugin**.
4. Selecciona el archivo `redes-sociales-plugin.zip` y haz clic en **Upload**.
5. Acepta el aviso de confianza. ¡Listo! Las 17 skills quedan disponibles.

> ⚠️ Sube el archivo **`redes-sociales-plugin.zip`** tal cual. No subas un zip que
> contenga otros zips adentro: Claude Desktop los rechaza con el error
> *"Zip cannot contain nested zip files"*.

---

## ⭐ IMPORTANTE: usa Cowork, no un chat normal

Trabaja siempre dentro de un proyecto de **Cowork**. Cowork tiene **memoria**:
guarda tus archivos (`about-me.md`, `voice.md`) y recuerda tu contexto entre
sesiones, así tu voz queda lista para siempre. En un chat normal todo se pierde
al cerrarlo.

## Empieza por la base ⭐

Pide **"construye mi voz"** primero. Crea los archivos `about-me.md` y `voice.md`
que **todas las demás skills necesitan** para escribir con tu voz.

## Configura las API keys (solo 2 skills)

La mayoría de las skills funcionan sin configuración. Estas dos necesitan claves
como variables de entorno:

| Skill | Variable de entorno | Para qué |
|---|---|---|
| `evaluador-de-publicaciones` | `APIFY_API_TOKEN` | Leer tu historial de LinkedIn |
| `guiones-de-reels` | `APIFY_API_TOKEN` + `GOOGLE_AI_API_KEY` | Analizar Reels con Gemini |

---

> 🚀 ¿Listo para usarlas? Mira **[COMO-USAR.md](COMO-USAR.md)**.
> 🤔 ¿Quieres saber qué es esto? Mira **[QUE-ES.md](QUE-ES.md)**.

## Lista completa de skills

| Skill | Qué hace |
|---|---|
| `constructor-de-voz` | Construye tu perfil de voz (`about-me.md` + `voice.md`). La base de todo. |
| `voz-de-newsletter` | Reglas de escritura específicas para el newsletter. |
| `optimizador-de-perfil` | Reconstruye tu perfil de LinkedIn para conversiones. |
| `redactor-de-publicaciones` | Redacta publicaciones de LinkedIn en tu voz. |
| `disenador-grafico` | Elige y crea un gráfico HTML/CSS o una infografía con IA. |
| `evaluador-de-publicaciones` | Evalúa borradores frente a tu historial real. |
| `guiones-de-reels` | Ingeniería inversa de un Reel + guion nuevo en tu voz. |
| `miniatura-de-youtube` | Convierte un título en un prompt de miniatura para Gemini. |
| `comentario-fijado` | Comentarios fijados estilo meme + prompt de imagen. |
| `generador-de-ganchos` | 6 variaciones de gancho por tema. |
| `formateador-de-publicaciones` | De un tema a una publicación usando PAS, AIDA, BAB, STAR o SLAY. |
| `matriz-de-contenido` | Pilares x formatos para 32+ ideas en una tabla. |
| `investigacion-de-nicho` | Investigación de nicho de 7 días con Claude for Chrome. |
| `infografia-gemini` | El estilo pizarra para Gemini. |
| `carrusel-gemini` | Generador de carrusel diapositiva por diapositiva. |
| `publicacion-con-cita` | Claude escribe la cita, Gemini recrea la imagen. |
| `panel-de-analiticas` | De una exportación de LinkedIn a un dashboard de React + 5 recomendaciones. |
