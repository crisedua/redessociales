---
name: guiones-de-reels
description: >
  Convierte un Reel de Instagram de referencia en un guion para tu propio Reel, ajustado a tu voz y reaprovechado a partir del contenido de tu newsletter. Recibe una URL de Reel o un enlace de referencia de Notion, usa Apify para extraer el video, lo envía a Gemini 2.5 Flash para obtener la transcripción completa + análisis del hook + estructura, y luego escribe un nuevo guion aplicando los mismos patrones al tema de tu newsletter. Usa esta skill cada vez que el usuario diga "haz un guion de reel", "guion para reels", "convierte esto en un reel", pegue una URL de Reel de Instagram o haga referencia a su base de datos de reels destacados (outliers) en Notion. Requiere las variables de entorno APIFY_API_TOKEN y GOOGLE_AI_API_KEY.
---

# Guion de Reels

## CRÍTICO: Auto-inicio al cargar

Cuando esta skill se active, ve directo al Paso 1. No resumas.

## Prerrequisitos

Esta skill necesita:

- Variable de entorno `APIFY_API_TOKEN` (extracción de Instagram)
- Variable de entorno `GOOGLE_AI_API_KEY` (análisis de video con Gemini 2.5 Flash)
- Node.js 18+ y los paquetes `apify-client` y `@google/generative-ai`

Si falta alguna de las variables de entorno, dile al usuario que ejecute:

```
! export APIFY_API_TOKEN=your_token
! export GOOGLE_AI_API_KEY=your_key
```

Luego detente hasta que ambas estén configuradas.

## Paso 1. Obtén la referencia

Pregunta:

> Pega la URL del Reel de referencia o el enlace de Notion. Este es el Reel destacado (outlier) al que quieres hacerle ingeniería inversa del formato.

Espera la URL.

Si el usuario pega un enlace de Notion, síguelo con WebFetch, ubica la URL del Reel de Instagram en la página y extráela. Si no se encuentra ninguna URL de Reel en la página de Notion, pídele al usuario que pegue la URL del Reel directamente.

## Paso 2. Obtén el tema del newsletter

Pregunta:

> ¿Cuál es el tema de tu newsletter que quieres reaprovechar para este Reel? Pega la sección relevante del newsletter, o escribe la idea central en una oración.

Espera el tema. Lee voz-de-newsletter.md, voice.md y about-me.md del proyecto si existen, para que el guion coincida con la voz del usuario.

## Paso 3. Extrae y descarga el Reel

Crea `~/Desktop/Reels/` si no existe. Escribe un script de Node.js en `~/Desktop/Reels/analyse-reel.js` que:

1. Use `apify-client` para llamar a `apify/instagram-reel-scraper` con `{ directUrls: [reelUrl], resultsLimit: 1 }`. Si eso no devuelve elementos, recurre a `{ urls: [reelUrl], resultsLimit: 1 }`, y luego a `apify/instagram-scraper` con `{ directUrls: [reelUrl], resultsType: 'posts', resultsLimit: 1 }`.
2. Extraiga `videoUrl` del elemento devuelto.
3. Descargue el video a `~/Desktop/Reels/downloads/{username}_{shortCode}.mp4`.
4. Guarde los datos crudos de la extracción en `~/Desktop/Reels/reel_data_{shortCode}.json`.

Ejecuta el script. Confirma el tamaño del archivo y los metadatos (vistas, likes, comentarios, primeros 200 caracteres del caption) antes de continuar.

## Paso 4. Analiza con Gemini 2.5 Flash

Extiende el script de Node (o ejecuta una segunda pasada) que:

1. Lea el `.mp4` descargado como base64.
2. Llame a `genAI.getGenerativeModel({ model: 'gemini-2.5-flash' })`.
3. Envíe el video con este prompt exacto:

```
Estoy estudiando este Reel para escribir mi propio guion en un estilo similar para mi audiencia de [AUDIENCIA DE about-me.md].

## Transcripción completa
- Transcribe CADA palabra con marcas de tiempo

## Hook
- Las primeras palabras exactas que se dicen
- Conteo de palabras del hook
- ¿Qué hace que detenga el scroll?

## Patrones de lenguaje
- Longitud promedio de las oraciones
- Proporción de "tú/ti" frente a "yo/mí"
- Transiciones entre puntos
- ¿Dónde están los minimizadores tipo "solo"?

## Estructura
- Duración total
- Desglose por secciones con tiempos
- ¿Cuál es el momento de antes/después?
- ¿Cuál es el CTA?

## Una idea clave
- La técnica más importante que se puede aprender de este Reel
```

Guarda el análisis en `~/Desktop/Reels/analysis_reference_{shortCode}.md`.

## Paso 5. Escribe el nuevo guion del Reel

Usando el análisis del Paso 4, el tema del newsletter del Paso 2 y los archivos de voz del usuario, escribe un nuevo guion de Reel en `~/Desktop/Reels/reel-[slug].md`.

Aplica estas reglas (no negociables):

### Hook
- Nunca abras con "Yo". Usa "esto", "tú", un hecho o la mención de un nombre.
- Formatos probados: "Esto cambió... para siempre" / giro negativo ("X no sirve a menos que...") / declaración de capacidad.
- El hook crea curiosidad o interrupción de patrón dentro de los primeros 3 segundos.
- Refleja el conteo de palabras y la estructura del hook del análisis de referencia.

### Cuerpo
- Español (Latinoamérica). Oraciones cortas. Sin rayas (em dashes), sin punto y coma.
- Usa "tú" y minimizadores tipo "solo" de forma conversacional ("solo lo conectas...").
- Nunca fusiones tres o más fragmentos staccato. Combínalos en una sola oración fluida.
- Nunca enuncies la conclusión. Deja que los hechos hagan el trabajo.
- Sin "enlace en la bio". Usa automatización de comentarios.

### Disparador de comentario (comment trigger)
- Una sola palabra en mayúsculas (GUION, WIKI, PROMPTS, VIDEO).
- Debe relacionarse directamente con lo que se promete.
- Sin comillas, sin "abajo", sin puntuación final.

### CTA
- "Comenta [PALABRA] y te envío [cosa específica]"
- Corto. Sin relleno tipo "el enlace a mi guía completa".

### Duración y estructura
- Apunta a 30 a 45 segundos en total.
- 2 puntos clave como máximo, no 3.
- El caption refleja el guion. Actualiza ambos juntos.

### Estructura del archivo del guion

```
# Reel: [título]

## Análisis de referencia
- URL: [url del reel]
- Vistas: [número]
- Técnica clave: [del análisis de Gemini]

## Duración objetivo
30-45 segundos

## Hook (0-3s)
[Palabras exactas]

## Punto 1 ([inicio]-[fin]s)
[Palabras exactas]

## Punto 2 ([inicio]-[fin]s)
[Palabras exactas]

## CTA ([inicio]-[fin]s)
[Palabras exactas, incluyendo "Comenta [PALABRA]"]

---

## Caption
[Refleja el guion, formateado para Instagram]

## Disparador de comentario
[PALABRA]

## Entregable
[Lo que desbloquea el disparador de comentario]

---

## Notas visuales
[Cortes, ideas de B-roll, textos en pantalla]
```

## Paso 6. Bucle de QA

Califica el guion contra las reglas del Paso 5. Toda violación debe corregirse. Vuelve a calificar hasta que el guion alcance 95/100. Nunca le muestres al usuario nada por debajo de 95.

Violaciones comunes a revisar:
- Abre con "Yo"
- Fragmentos staccato de tres o más
- Enuncia la conclusión
- Disparador de comentario de varias palabras o estilizado
- Duración mayor a 45 segundos al leerlo en voz alta
- 3 puntos en lugar de 2
- El caption no refleja el guion

## Paso 7. Ofrece el pipeline

Después de que el guion sea aprobado, ofrece:

> Dos caminos desde aquí:
>
> 1. Grábalo tú mismo.
> 2. Genéralo automáticamente con ElevenLabs (voz) + HeyGen (avatar) + Remotion (motion graphics). Si tienes el proyecto my-video configurado, ejecuta `npm run pipeline:claude-routines` con la configuración de este guion.

## Reglas

- Nunca te saltes la barrera de QA de 95/100.
- Siempre lee voice.md y about-me.md antes de escribir. La coincidencia de voz no es negociable.
- Nunca inventes métricas del Reel de referencia. Usa solo lo que devuelve Apify.
- Español (Latinoamérica). Sin rayas (em dashes). Sin punto y coma.
- Cada entregable de guion incluye el caption exacto y el disparador de comentario junto al guion. Nunca entregues solo el guion.
- Si la extracción del Reel de referencia falla en las tres variantes de actor, reporta el fallo y detente. No fabriques análisis.
- Gemini 2.5 Flash es el modelo. No lo sustituyas sin la aprobación del usuario.
