# 📦 Cómo instalar las Skills de Redes Sociales en Claude Desktop

Hay dos formas de instalarlas. La **Opción A (plugin)** es la más rápida porque
instala las 17 skills de una sola vez.

---

## ✅ Opción A — Subir como plugin (recomendada)

Instala todas las skills juntas con un solo archivo.

1. Descarga **[`dist/redes-sociales-plugin.zip`](dist/redes-sociales-plugin.zip)**
   desde este repositorio (ábrelo y usa el botón **Download**).
2. Abre **Claude Desktop** → **Configuración (Settings)** → **Plugins**.
3. Ve a la pestaña **Local uploads** y haz clic en **+** → **Upload local plugin**.
4. Selecciona el archivo `redes-sociales-plugin.zip` y haz clic en **Upload**.
5. Acepta el aviso de confianza. ¡Listo! Las 17 skills quedan disponibles.

> ⚠️ Sube el archivo `redes-sociales-plugin.zip` tal cual. No subas
> `todas-las-skills.zip` ni un zip que contenga otros zips adentro: Claude
> Desktop los rechaza con el error *"Zip cannot contain nested zip files"*.

---

## Opción B — Subir skills una por una

Si prefieres elegir solo algunas skills:

1. En la carpeta [`dist/`](dist/) descarga las skills que quieras
   (por ejemplo `constructor-de-voz.zip`).
2. En **Claude Desktop** → **Configuración** → **Capacidades (Capabilities)** →
   **Skills**, haz clic en **Subir skill** y selecciona el `.zip`.
3. Repite por cada skill (una skill por archivo).

---

## Empieza por la base ⭐

Ejecuta **`constructor-de-voz`** primero. Crea los archivos `about-me.md`
y `voice.md` que **todas las demás skills necesitan** para escribir con tu voz.

## Configura las API keys (solo 2 skills)

La mayoría de las skills funcionan sin configuración. Estas dos necesitan claves
como variables de entorno:

| Skill | Variable de entorno | Para qué |
|---|---|---|
| `evaluador-de-publicaciones` | `APIFY_API_TOKEN` | Leer tu historial de LinkedIn |
| `guiones-de-reels` | `APIFY_API_TOKEN` + `GOOGLE_AI_API_KEY` | Analizar Reels con Gemini |

## Úsalas

Pídele a Claude en español y elegirá la skill correcta:

- *"Construye mi voz"* → `constructor-de-voz`
- *"Escríbeme una publicación sobre IA"* → `redactor-de-publicaciones`
- *"Evalúa este borrador frente a mi historial"* → `evaluador-de-publicaciones`
- *"Hazme un carrusel a partir de esto"* → `carrusel-gemini`
- *"Qué debería publicar esta semana"* → `investigacion-de-nicho` o `matriz-de-contenido`
- *"Convierte este Reel destacado en un guion"* → `guiones-de-reels`
- *"Necesito una miniatura para mi video"* → `miniatura-de-youtube`
- *"Escríbeme un comentario fijado"* → `comentario-fijado`

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
