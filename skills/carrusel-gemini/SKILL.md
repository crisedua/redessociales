---
name: carrusel-gemini
description: >
  Genera un carrusel de LinkedIn con tu marca, diapositiva por diapositiva, usando Gemini. Toma contenido fuente, arma un brief de diseño, espera aprobación y luego entrega prompts de generación de imágenes por diapositiva. Formato vertical 1080x1350. Usa esta skill siempre que el usuario diga "carrusel", "hazme un carrusel", "convierte esto en un carrusel", "carrusel gemini", o quiera contenido de LinkedIn de varias diapositivas. Incluye siempre un punto de aprobación entre el brief y la generación de imágenes.
---

# Gemini Carousel

## CRÍTICO: Inicio automático al cargar

Cuando esta skill se active, ve directo al Paso 1. No resumas.

## Paso 1. Reúne los insumos

Pregunta:

> Pega el contenido que quieres en el carrusel. Sirve una publicación, una sección de newsletter, notas de investigación o un framework.

Espera el contenido y luego llama a AskUserQuestion:

```json
[
  {
    "question": "¿Estilo de marca?",
    "header": "Estilo",
    "multiSelect": false,
    "options": [
      {"label": "Tomar de brand-kit.md", "description": "Usar los colores y la tipografía de mi archivo de marca del proyecto"},
      {"label": "Yo escribiré los colores de marca", "description": "Pegaré códigos hex y preferencias de fuentes"},
      {"label": "Sugiéreme tú", "description": "Elige una paleta y tipografía según el contenido"}
    ]
  },
  {
    "question": "¿Cuántas diapositivas?",
    "header": "Diapositivas",
    "multiSelect": false,
    "options": [
      {"label": "6 diapositivas", "description": "Conciso, lectura rápida"},
      {"label": "8 diapositivas", "description": "Longitud estándar de carrusel"},
      {"label": "10 diapositivas", "description": "Carrusel a fondo"}
    ]
  }
]
```

## Paso 2. Arma el brief de diseño

Analiza el contenido y produce un brief diapositiva por diapositiva con:

- **Diapositiva 1 (Portada)**: gancho, texto grande en negrita, dirección visual
- **Diapositivas 2 a N-1 (Cuerpo)**: una idea por diapositiva, máximo 15 palabras por diapositiva, sugerencia visual
- **Diapositiva N (CTA)**: pedido de repost, nombre, enlace u oferta

Para cada diapositiva incluye:

- Número de diapositiva
- Titular (máximo 8 palabras)
- Texto del cuerpo (máximo 15 palabras)
- Sugerencia visual (ícono, bloque de color, ilustración, diagrama)

Dile al usuario:

> Aquí está el brief de diseño. Dime qué cambiar, o di "generar" cuando estés conforme.

Espera la aprobación. No avances hasta que el usuario apruebe explícitamente.

## Paso 3. Entrega los prompts por diapositiva

Una vez aprobado, entrega un prompt de generación de imágenes de Gemini por diapositiva, cada uno en su propio bloque de código, numerado con claridad.

Cada prompt sigue esta estructura:

```
Actúa como un diseñador gráfico experto. Crea una diapositiva de carrusel de LinkedIn de 1080x1350 píxeles (relación de aspecto 4:5).

Estilo de marca:
- Color primario: [HEX]
- Color secundario: [HEX]
- Color de acento: [HEX]
- Tipografía: [fuente de titular industrial en negrita, fuente de cuerpo geométrica y limpia]
- Estética: moderna, autoritaria, de alto contraste

Diapositiva [N de M]: [propósito de la diapositiva]

Contenido:
- Titular: "[texto del titular]"
- Cuerpo: "[texto del cuerpo]"
- Elemento visual: [sugerencia visual específica]

Instrucciones de layout:
- [Ubicación y tamaño del titular]
- [Ubicación y tamaño del cuerpo]
- [Ubicación del visual]
- [Tratamiento del fondo]

Restricciones:
- Relación de aspecto vertical 4:5 a exactamente 1080x1350 píxeles
- Sin marcas de agua, sin logos a menos que se especifique arriba
- Mantén la consistencia visual con las demás diapositivas del set
```

Dile al usuario:

> Pega cada prompt en un chat nuevo de Gemini con Create Image activado y Nano Banana seleccionado. Genera las diapositivas una a una para máximo control sobre la consistencia.

## Paso 4. Ofrece la alternativa de una sola pasada

Después de los prompts por diapositiva, ofrece:

> ¿Quieres un único prompt combinado que genere todo el carrusel de una sola pasada? Es más rápido pero con menos consistencia visual. Di "combinar" y lo reescribo.

## Reglas

- Pasa siempre por el punto de aprobación del brief por parte del usuario antes de entregar los prompts de imagen.
- 1080x1350 píxeles por diapositiva. Ninguna otra relación de aspecto.
- Máximo 15 palabras de texto de cuerpo por diapositiva. La legibilidad se pierde en el feed.
- Mantén el estilo de marca idéntico en cada prompt de diapositiva para que el set parezca un solo carrusel.
- La diapositiva de portada (1) y la diapositiva de CTA (última) deben ser visualmente distintas de las diapositivas de cuerpo.
- Nunca uses guiones largos (em dashes).
- Español neutro a menos que voice.md indique lo contrario.
- Si existe brand-kit.md en el proyecto, léelo y usa sus códigos hex y elecciones tipográficas exactas.
