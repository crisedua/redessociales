# 📦 Cómo instalar las Skills de Redes Sociales en Claude Desktop

Esta guía explica cómo descargar las skills e instalarlas en **Claude Desktop**
(la app de escritorio para Mac/Windows).

## 1. Descarga las skills

**Opción rápida (todas a la vez):**
- Ve a la carpeta [`dist/`](dist/) de este repositorio → abre **`todas-las-skills.zip`** → botón **Download**.
- Descomprime el archivo. Obtendrás 17 archivos `.zip`, uno por skill.

**Opción individual (solo las que quieras):**
- En [`dist/`](dist/), haz clic en la skill que necesites (por ejemplo `constructor-de-voz.zip`) → **Download**.

## 2. Súbelas a Claude Desktop

1. Abre **Claude Desktop**.
2. Ve a **Configuración (Settings)** → **Capacidades (Capabilities)** → **Skills**.
3. Haz clic en **Subir skill (Upload skill)** o en el botón **+**.
4. Selecciona uno de los archivos `.zip`.
5. Repite para cada skill (Claude Desktop sube **una skill por archivo**).

## 3. Empieza por la base ⭐

Sube y ejecuta **`constructor-de-voz`** primero. Crea los archivos `about-me.md`
y `voice.md` que **todas las demás skills necesitan** para escribir con tu voz.

## 4. Configura las API keys (solo 2 skills)

La mayoría de las skills funcionan sin configuración. Estas dos necesitan claves
como variables de entorno:

| Skill | Variable de entorno | Para qué |
|---|---|---|
| `evaluador-de-publicaciones` | `APIFY_API_TOKEN` | Leer tu historial de LinkedIn |
| `guiones-de-reels` | `APIFY_API_TOKEN` + `GOOGLE_AI_API_KEY` | Analizar Reels con Gemini |

## 5. Úsalas

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
