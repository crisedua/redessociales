---
name: infografia-gemini
description: >
  Genera el prompt de infografía estilo pizarra dibujada a mano que consiguió 480k impresiones en 3 publicaciones. Toma contenido fuente (una publicación, newsletter, blog, nota de investigación) y devuelve un prompt completo de generación de imágenes para Gemini con un brief estructurado. Usa esta skill siempre que el usuario diga "infografía de pizarra", "infografía gemini", "gráfico dibujado a mano", "convierte esto en una pizarra", o quiera una infografía generada con IA para una publicación.
---

# Gemini Infographic

## CRÍTICO: Inicio automático al cargar

Cuando esta skill se active, ve directo al Paso 1. No resumas el proceso.

## Paso 1. Obtén el contenido fuente

Pregunta:

> Pega el contenido que quieres convertir en infografía. Sirve una publicación, una sección de newsletter, un blog, una nota de investigación o puntos sueltos.

Espera el contenido.

## Paso 2. Arma el brief

Analiza el contenido y produce un brief de infografía en lenguaje sencillo. Incluye:

- **Título** (6 palabras o menos, contundente)
- **Subtítulo** (opcional, una línea de contexto)
- **Estructura central**: decide entre pasos, framework, comparación, estadísticas o lista
- **Puntos clave**: máximo 3 a 7 viñetas, cada una de 10 palabras o menos
- **Sugerencias visuales**: flechas, recuadros, números resaltados, íconos, acentos de color. Sé específico sobre la ubicación y el color.
- **CTA del pie**: texto a mano que dice "Sigue a [Nombre] [Tagline] para más contenido útil | Reposta ♻️"

Dile al usuario:

> Aquí está el brief. Dime qué cambiar, o di "generar" cuando estés conforme.

Espera la aprobación.

## Paso 3. Entrega el prompt de Gemini

Una vez aprobado, entrega el prompt completo en un bloque de código, con el brief insertado en el marcador `[INSERTA AQUÍ EL CONTENIDO Y EL LAYOUT DE TU INFOGRAFÍA]`:

```
Genera una sola imagen de una infografía física, dibujada a mano sobre una pizarra grande o una página de cuaderno.

Instrucciones de estilo cruciales (lee esto primero):

Medio: La imagen debe parecer una fotografía de una pizarra real o de un bloc de papel grande.

Textura: Todos los elementos deben verse hechos a mano con marcadores de colores (negro, azul, rojo, verde) y resaltadores (amarillo/naranja). Las líneas deben ser ligeramente imperfectas, temblorosas y tener la textura de tinta sobre una superficie.

Sin fuentes digitales: Todo el texto, los títulos y las viñetas deben verse escritos o impresos a mano con marcador.

Layout: Estructura la imagen de 1080x1350 de la siguiente manera:

[INSERTA AQUÍ EL BRIEF — título, subtítulo, estructura central, puntos clave, sugerencias visuales]

Usa marcadores de varios colores para dar énfasis. Mantén el texto grande y legible. Haz que todo se vea dibujado a mano con leves imperfecciones. Haz que parezca una fotografía de una página de cuaderno real.

Incluye siempre el texto a mano "Sigue a [Nombre] [Tagline] para más contenido útil | Reposta ♻️" en la parte inferior de la imagen, con el mismo estilo de marcador dibujado a mano.
```

Dile al usuario:

> Pega esto en un chat nuevo de Gemini con Create Image activado y Nano Banana seleccionado. Genera en 1080x1350.

## Paso 4. Ofrece iteración

Después del prompt, ofrece:

> Si la primera generación no da en el blanco, dime qué ajustar y reescribiré el prompt. Arreglos comunes: menos colores, título más grande, otra dirección de layout.

## Reglas

- La salida de 1080x1350 píxeles es innegociable. El formato vertical domina el feed de LinkedIn.
- El CTA del pie siempre incluye el símbolo de reciclaje y "Reposta".
- Nunca uses guiones largos (em dashes) en ninguna salida.
- Mantén las viñetas en menos de 10 palabras. El texto más largo pierde legibilidad a la escala de la pizarra.
- Espera siempre la aprobación del brief por parte del usuario antes de entregar el prompt final.
- Español neutro a menos que voice.md indique lo contrario.
- Si el usuario tiene brand-kit.md o colours.md en el proyecto, integra sus colores de marca en las sugerencias visuales.
