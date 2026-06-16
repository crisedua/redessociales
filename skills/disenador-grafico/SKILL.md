---
name: disenador-grafico
description: >
  Crea gráficos para publicaciones de LinkedIn. Decide entre un gráfico estructurado en HTML/CSS o una infografía generada con IA según el contenido de la publicación. Usa esta skill cuando el usuario diga "diseña un gráfico", "crea un visual", "haz una imagen", "gráfico para mi publicación", "imagen de LinkedIn" o quiera cualquier contenido visual para acompañar una publicación de LinkedIn. Actívala también cuando el usuario termine de escribir una publicación y quiera un gráfico que combine.
---

# Graphic Designer

## CRÍTICO: Inicio automático al cargar

Cuando se active esta skill, ve directo al Paso 1. No resumas. No expliques las opciones. Empieza de inmediato.

## Paso 1. Lee la publicación

Revisa el proyecto en busca del archivo de publicación más reciente. Si lo encuentras, léelo. Si no, di:

> Pega la publicación para la que quieres un gráfico.

Espera la publicación y luego llama a AskUserQuestion:

```json
[
  {
    "question": "¿Qué tipo de gráfico encaja mejor con esta publicación?",
    "header": "Estilo",
    "multiSelect": false,
    "options": [
      {"label": "Gráfico HTML/CSS", "description": "Diseño estructurado y limpio. Framework, comparación, pasos, datos. Totalmente editable, se exporta con captura de pantalla."},
      {"label": "Infografía de pizarra", "description": "Estilo de marcador dibujado a mano sobre una pizarra o página de cuaderno. Resume la publicación visualmente. Generada en Gemini."},
      {"label": "Infografía con marca", "description": "Infografía profesional con los colores de tu marca. Resume la publicación visualmente. Generada en Gemini."},
      {"label": "Tú decides", "description": "Analiza la publicación y elige el mejor formato automáticamente"}
    ]
  }
]
```

Si elige "Tú decides": analiza la publicación. Si contiene pasos numerados, frameworks, comparaciones o tablas de datos, ve por la Ruta A (HTML/CSS). Si resume un flujo de trabajo, comparte consejos, enseña un concepto o cuenta una historia, ve por la Ruta B (prompt de imagen) y elige el estilo que mejor encaje.

## Ruta A: Gráfico estructurado en HTML/CSS

Restricciones de diseño:
- 1200 x 1400 píxeles (óptimo para LinkedIn)
- Fondo oscuro (#1a1a2e o el color de marca del usuario) con texto de alto contraste
- Fuente sans-serif limpia (Inter, system-ui)
- Texto blanco o claro sobre fondo oscuro
- Un color de acento para resaltados y divisores
- Mínimo 40px de relleno (padding) en todos los lados
- Sin fondos de fotos de stock
- Deja que el contenido de la publicación determine cuántas secciones tiene el gráfico. 3 pasos = 3 bloques. 10 consejos = 10 bloques. La restricción es la legibilidad, no un número fijo. Cada elemento debe ser lo bastante grande para leerse en una pantalla de móvil.

Un único archivo HTML autónomo con CSS en línea. Incluye la etiqueta meta viewport.

Extrae el framework o los pasos centrales de la publicación. No copies la publicación completa. Resume en:
- Un titular corto (5 a 8 palabras)
- Puntos clave como bloques visuales (los iconos Unicode están bien)
- Un pie con el nombre del autor de about-me.md si está disponible

Guarda el archivo HTML. Dile al usuario:

> Abre el HTML en tu navegador y tómale una captura de pantalla.

## Ruta B: Prompt de generación de imágenes

El gráfico debe resumir el contenido de la publicación visualmente. No es una ilustración abstracta ni una foto de stock. Resume la información clave de la publicación en un formato visual que el lector pueda escanear.

Primero, extrae el contenido para la infografía a partir de la publicación:
- El titular o gancho principal (acortado a 5 a 10 palabras)
- De 3 a 6 puntos clave, pasos o aprendizajes (una línea corta cada uno)
- Cualquier número, estadística o dato que valga la pena resaltar
- Una línea de pie (nombre del autor y CTA si corresponde)

Luego construye el prompt según el estilo elegido.

### Estilo 1: Infografía de pizarra

Usa esta plantilla de prompt. Completa las secciones de contenido a partir de la publicación.

```
Genera una sola imagen de una infografía física, dibujada a mano, sobre una pizarra grande o una página de cuaderno.

Instrucciones de estilo cruciales (léelas primero):
Medio: La imagen debe verse como una fotografía de una pizarra real o un bloc de papel grande.
Textura: Todos los elementos deben verse hechos a mano con marcadores de colores (negro, azul, rojo, verde) y resaltadores (amarillo/naranja). Las líneas deben ser ligeramente imperfectas, temblorosas y tener la textura de la tinta sobre una superficie.
Sin fuentes digitales: Todo el texto, los títulos y los puntos deben verse escritos o impresos a mano con marcador.

Diseño: Estructura la imagen de 1080x1350 de la siguiente manera:

TÍTULO (marcador grande y en negrita, parte superior de la página):
[Inserta el titular de la publicación]

CONTENIDO (secciones dibujadas a mano con marcador):
[Inserta de 3 a 6 puntos clave, cada uno como una línea corta escrita a mano con una viñeta, número o un pequeño icono dibujado al lado]

[Si hay estadísticas o números, dibújalos en grande con un círculo o recuadro alrededor]

Usa marcadores de varios colores para dar énfasis. Mantén el texto grande y legible. Haz que todo se vea dibujado a mano con ligeras imperfecciones. Haz que parezca una fotografía de una página de cuaderno real.

Incluye siempre el texto manuscrito "[Nombre del autor de about-me.md] | Repost" en la parte inferior de la imagen, con el mismo estilo de marcador dibujado a mano.
```

### Estilo 2: Infografía con marca

Pregunta al usuario por los colores de marca si aún no los conoces. Si existe about-me.md, revísalo primero.

```
Genera una imagen de infografía profesional a 1080x1350 píxeles.

Estilo: Limpio, moderno, editorial. Diseño plano (flat) con bordes nítidos y tipografía fuerte. Sin efectos 3D, sin degradados, sin fotos de stock.

Paleta de colores:
- Fondo: [color de marca primario o neutro oscuro]
- Texto: [blanco o color de alto contraste]
- Acento: [color de marca secundario]

Diseño:
TITULAR (parte superior, texto grande y en negrita):
[Inserta el titular de la publicación]

CUERPO (secciones estructuradas, cada una con un icono o número):
[Inserta de 3 a 6 puntos clave como líneas cortas, cada una con un marcador visual: círculo numerado, checkmark o un icono simple]

[Si hay estadísticas, muéstralas como números destacados grandes con una etiqueta debajo]

PIE:
[Nombre del autor de about-me.md] | [CTA o eslogan si corresponde]

Mantén el texto grande y escaneable. Máximo 40 palabras en toda la imagen. Sin bordes decorativos. Sin marcas de agua. Sin logotipos a menos que el usuario proporcione uno.
```

Entrega el prompt completo en un bloque de código. Dile al usuario:

> Pega esto en Gemini o en tu generador de imágenes. El prompt está listo para usar.

## Después de cualquiera de las rutas

Di:

> Gráfico listo. Di "califica mi publicación" cuando quieras feedback antes de publicar.

## Reglas

- Siempre lee la publicación antes de diseñar. El gráfico debe resumir el contenido de la publicación, no ilustrar un concepto abstracto.
- Los gráficos estructurados (Ruta A) deben ser un único archivo HTML con CSS en línea.
- Los prompts de imagen (Ruta B) deben ser totalmente autónomos. El usuario lo pega en frío en Gemini y obtiene el gráfico.
- Extrae y resume el contenido de la publicación en el gráfico. No copies el texto completo de la publicación.
- Estilo de pizarra: siempre con aspecto de marcador dibujado a mano, líneas imperfectas, plumones de colores, textura de cuaderno/pizarra.
- Estilo con marca: siempre limpio, plano, moderno, usando los colores de marca del usuario.
- Nunca uses rayas largas (em dashes) en ninguna salida.
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante.
