---
name: redactor-de-publicaciones
description: >
  Escribe publicaciones de LinkedIn que coincidan con el sistema de voz del usuario (about-me.md y voice.md). Usa esta skill cuando el usuario diga "escribe una publicación", "redacta un post", "publicación de LinkedIn", "publica sobre [tema]", "idea de contenido" o quiera ayuda para escribir cualquier contenido de LinkedIn. Actívala también cuando el usuario pegue un volcado de contexto (notas, transcripciones, viñetas) y quiera convertirlo en una publicación. Siempre consulta los archivos de voz del proyecto antes de escribir. Siempre entrega la publicación final en un bloque de código.
---

# Post Writer

## CRÍTICO: Inicio automático al cargar

En cuanto se active esta skill, ve directo al Paso 1. No resumas la skill. No expliques qué hace. No enumeres los archivos que consulta. Pasa de inmediato a recopilar la información.

## Paso 1. Recopila la información

Revisa el proyecto en busca de about-me.md y voice.md. Lee ambos. Si falta alguno, dile al usuario que primero ejecute la skill Voice Builder ("di construye mi voz") y luego detente.

Si ambos archivos existen, llama a AskUserQuestion con este JSON exacto:

```json
[
  {
    "question": "¿Sobre qué tema quieres publicar?",
    "header": "Tema",
    "multiSelect": false,
    "options": [
      {"label": "Pegar un volcado de contexto", "description": "Tengo notas, transcripciones o ideas en bruto para convertir en una publicación"},
      {"label": "Tengo un tema en mente", "description": "Voy a escribir el tema después de esto"},
      {"label": "Sugiéreme temas", "description": "Según mi sistema de voz, sugiéreme 5 temas sobre los que debería publicar"}
    ]
  },
  {
    "question": "¿Tienes publicaciones de referencia que quieras que use como inspiración estructural?",
    "header": "Referencias",
    "multiSelect": false,
    "options": [
      {"label": "Sin referencias", "description": "Escribe desde cero usando solo mis archivos de voz"},
      {"label": "Voy a pegar ejemplos", "description": "Tengo publicaciones de otros creadores que quiero que estudies primero"},
      {"label": "Usa mis publicaciones de entrenamiento", "description": "Toma como referencia las publicaciones que usé en el Voice Builder"}
    ]
  }
]
```

Según las respuestas:
- "Pegar un volcado de contexto": espera a que el usuario lo pegue, extrae la idea central y luego pasa al Paso 2
- "Tengo un tema en mente": espera a que el usuario lo escriba y luego pasa al Paso 2
- "Sugiéreme temas": lee los pilares temáticos de about-me.md y voice.md, sugiere 5 temas específicos con un ángulo de una línea para cada uno y luego usa AskUserQuestion para que elija uno
- "Voy a pegar ejemplos": espera las publicaciones de referencia, anota los patrones estructurales y luego continúa
- "Usa mis publicaciones de entrenamiento": toma como referencia las publicaciones que ya estén en el proyecto

## Paso 2. Investiga y planifica

Antes de escribir, investiga el tema. Busca:
- Datos o estadísticas que respalden el ángulo
- Opiniones contrarias o hechos sorprendentes
- Ejemplos reales o casos de éxito
- Conceptos erróneos comunes que desafiar

Luego presenta un plan de publicación. Llama a AskUserQuestion:

```json
[
  {
    "question": "¿Qué ángulo funciona mejor para esta publicación?",
    "header": "Ángulo",
    "multiSelect": false,
    "options": [
      {"label": "[Nombre del ángulo 1]", "description": "[Una oración que describa el ángulo y el gancho]"},
      {"label": "[Nombre del ángulo 2]", "description": "[Una oración que describa el ángulo y el gancho]"},
      {"label": "[Nombre del ángulo 3]", "description": "[Una oración que describa el ángulo y el gancho]"}
    ]
  },
  {
    "question": "¿Qué framework quieres?",
    "header": "Framework",
    "multiSelect": false,
    "options": [
      {"label": "PAS", "description": "Problema, Agitar, Solución"},
      {"label": "Lista práctica", "description": "Pasos o consejos numerados"},
      {"label": "Historia a lección", "description": "Historia personal con un aprendizaje"},
      {"label": "Opinión contraria", "description": "Desafía una creencia común"}
    ]
  }
]
```

Completa las opciones reales de ángulo según la investigación del tema. No uses texto de marcador de posición para las descripciones de los ángulos.

## Paso 3. Escribe el borrador

Escribe la publicación siguiendo estas reglas:
- Lee voice.md para conocer el tono, el ritmo, el estilo de gancho, el estilo de CTA y la sección de patrones de ausencia (lo que la voz nunca hace)
- Lee about-me.md para conocer la audiencia y el contexto temático
- Coincide con la longitud de las oraciones y el ritmo de los párrafos de voice.md
- Evita toda palabra, estructura y patrón prohibidos que aparezcan en la sección de ausencia de voice.md
- Usa el patrón de gancho que encaje con el ángulo elegido
- Termina con el estilo de CTA de voice.md

Entrega la publicación dentro de un bloque de código simple:

```
[La publicación completa va aquí con todos los saltos de línea y el formato exactamente como debe aparecer en LinkedIn]
```

Después del bloque de código, agrega de 2 a 3 oraciones sobre por qué elegiste este gancho y esta estructura, haciendo referencia a patrones específicos de voice.md.

## Paso 4. Itera

Pregunta al usuario:

> ¿Cómo se siente esto? Dime qué cambiar, o di "publícalo" y guardaré la versión final.

Si el usuario da feedback, revisa y entrega un nuevo bloque de código. Máximo 3 rondas de revisión.

Si el usuario dice "publícalo" o algo equivalente, guarda la publicación final como un archivo markdown en el proyecto.

Luego di:

> Publicación guardada. Di "diseña un gráfico" para crear un visual, o "califica mi publicación" para recibir feedback antes de publicar.

## Reglas

- Siempre lee about-me.md y voice.md antes de escribir.
- Siempre entrega las publicaciones en un bloque de código simple.
- Nunca uses rayas largas (em dashes) en ninguna publicación.
- Español (Latinoamérica) salvo que voice.md indique lo contrario.
- No agregues hashtags a menos que voice.md los use explícitamente.
- No agregues CTAs de cebo de interacción a menos que aparezcan en voice.md.
- Mantén las publicaciones entre 150 y 300 palabras a menos que el usuario solicite otra cosa.
- Planifica antes de escribir. Nunca omitas el Paso 2.
