---
name: formateador-de-publicaciones
description: >
  Convierte un tema en una publicación de LinkedIn lista para publicar usando los frameworks PAS, AIDA, BAB, STAR o SLAY. De 200 a 250 palabras, 20 líneas como máximo, con formato para móvil y líneas en blanco entre oraciones. Usa esta skill cada vez que el usuario diga "formatea esto como publicación", "convierte esto en una publicación de LinkedIn", "escríbelo en PAS" o cualquier framework con nombre, o cuando quiera una publicación bien estructurada a partir de un tema. Se diferencia de redactor-de-publicaciones: formateador-de-publicaciones aplica un framework estricto. redactor-de-publicaciones redacta en la voz del usuario sin restricciones de framework.
---

# Post Formatter

## IMPORTANTE: Inicio automático al cargar

Cuando esta skill se active, ve directamente al Paso 1. No resumas. Comienza de inmediato a recopilar los datos de entrada.

## Paso 1. Recopila los datos de entrada

Llama a AskUserQuestion:

```json
[
  {
    "question": "¿Sobre qué tema quieres publicar?",
    "header": "Tema",
    "multiSelect": false,
    "options": [
      {"label": "Voy a escribir el tema", "description": "Una sola oración que describa el asunto"},
      {"label": "Pegar un volcado de contexto", "description": "Notas, estadísticas o transcripciones para convertir en una publicación"}
    ]
  },
  {
    "question": "¿Qué framework?",
    "header": "Framework",
    "multiSelect": false,
    "options": [
      {"label": "PAS", "description": "Problema, Agitación, Solución"},
      {"label": "AIDA", "description": "Atención, Interés, Deseo, Acción"},
      {"label": "BAB", "description": "Antes, Después, Puente"},
      {"label": "STAR", "description": "Situación, Tarea, Acción, Resultado"},
      {"label": "SLAY", "description": "Historia, Lección, Consejo accionable, Tú"},
      {"label": "Elige por mí", "description": "Recomienda el mejor framework según el tema"}
    ]
  }
]
```

Haz una sola pregunta de seguimiento:

> ¿Algo más que deba saber? Datos, estadísticas, notas de tono o para quién es esto.

Espera la respuesta.

## Paso 2. Escribe la publicación

Aplica estas reglas globales a cada resultado:

- Máximo 20 líneas, de 200 a 250 palabras en total (~1.200 caracteres)
- Línea en blanco después de cada línea
- La mayoría de las líneas: una oración, de 55 caracteres o menos
- Hasta 4 líneas pueden ser miniparágrafos (de 2 a 3 oraciones, 110 caracteres o menos)
- Palabras de nivel sexto grado. Cero adverbios, cero jerga, cero relleno
- Sin rayas (em dash)
- Sin preguntas, salvo que el gancho mismo sea una pregunta
- Sin emojis, excepto palomitas para listas numeradas (1. 2. 3.) y el símbolo de reciclaje en el CTA
- Regla de tres: usa como máximo dos tríos por publicación
- Varía los inicios de oración. No abuses de "Yo"

## Paso 3. Estructura

- **Línea 1 (Gancho)**: En negrita. 50 caracteres o menos.
- **Línea 2 (Giro / Contraste)**: 50 caracteres o menos. Se opone al gancho o lo sorprende.
- **Líneas 3 a 18 (Núcleo)**: El framework elegido, repartido en 3 a 5 líneas por etapa. Cualquier lista dentro de una etapa debe tener exactamente tres elementos (1. 2. 3.). Usa flechas para mostrar el flujo donde sea útil.

Mapas de frameworks:

- **PAS**: Problema -> Agitación -> Solución
- **AIDA**: Atención -> Interés -> Deseo -> Acción
- **BAB**: Antes -> Después -> Puente
- **STAR**: Situación -> Tarea -> Acción -> Resultado
- **SLAY**: Historia -> Lección -> Consejo accionable -> Tú

- **Líneas 19 a 20 (Cierre y CTA)**: De 2 a 3 líneas que fijen la lección. Cierra con una de estas frases seguida del símbolo de reciclaje: "Comparte si", "Comparte esto" o "Si te sirvió, compártelo".

## Paso 4. Resultado

Entrega la publicación terminada dentro de un bloque de código. Sin preámbulo, sin notas al final.

## Paso 5. Ofrece el siguiente paso

Después de la publicación, pregunta:

> ¿Quieres un gráfico que combine (skill disenador-grafico) o quieres que la califique contra tu historial de publicaciones (skill evaluador-de-publicaciones)?

## Reglas

- Devuelve solo la publicación terminada. Sin metacomentarios.
- Haz cumplir los límites de longitud de línea, conteo de palabras y conteo de líneas. Cuéntalos.
- Nunca uses rayas (em dash).
- Español (Latinoamérica) en todo momento, salvo que voice.md especifique lo contrario.
- Si el usuario tiene voice.md en el proyecto, ajusta el tono y el ritmo para que coincidan.
- Si se usa un trío, debe tener exactamente tres elementos. Ni dos, ni cuatro.
