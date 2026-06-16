---
name: constructor-de-voz
description: >
  Construye un perfil de voz personalizado dentro de un proyecto de Cowork a partir de una entrevista breve y de 3 a 5 muestras de escritura. Funciona para cualquier formato de contenido: publicaciones de LinkedIn, newsletters, ensayos, correos, entradas de blog, tuits o cualquier otra escritura publicada. Usa esta skill al inicio de cualquier proyecto de Cowork donde el usuario quiera que Claude aprenda quién es y cómo escribe antes de redactar contenido nuevo. Actívala cada vez que el usuario diga "construye mi voz", "aprende mi voz", "configura mi sistema de contenido", "haz mi onboarding", "aprende de mi escritura", "aprende de mis publicaciones", "quiero que Claude suene como yo", o cuando deje un lote de muestras de escritura en el chat al inicio de un proyecto. Actívala también para usuarios de Cowork primerizos que necesiten una base de voz antes de escribir cualquier cosa. Siempre produce dos archivos (about-me.md y voice.md) guardados en la raíz del proyecto.
---

# Voice Builder

## CRÍTICO: Inicio automático al cargar

En el momento en que esta skill se carga, instala, sube o activa, DEBES ejecutar de inmediato el Paso 1 que aparece abajo. Esto significa que tu siguiente mensaje al usuario son las preguntas de la entrevista. Nada más.

NO debes:
- Resumir esta skill
- Describir qué archivos crea
- Explicar cómo funciona
- Decir "esto es lo que contiene esta skill"
- Preguntar si el usuario quiere ejecutarla
- Confirmar la instalación
- Ofrecer opciones como "¿quieres que la ejecute ahora?"

Haz ESTO:
- Ve directo al Paso 1
- Envía las preguntas de la entrevista como tu primera y única respuesta

Esto aplica sin importar si el usuario subió un archivo .skill, dijo "construye mi voz", pegó muestras o activó la skill de cualquier otra forma. Sin preámbulo. Sin resumen. Primero la entrevista.

## Paso 1. Realiza la entrevista de Sobre mí

DEBES llamar a la herramienta AskUserQuestion para hacer estas preguntas. No escribas las preguntas como texto del chat. Usa la herramienta. La herramienta se muestra como un formulario interactivo que el usuario completa, lo cual es una mejor experiencia que escribir respuestas en el chat.

AskUserQuestion admite un máximo de 4 preguntas por llamada, así que envía dos llamadas: primero el Lote 1, espera las respuestas, y luego el Lote 2.

### Lote 1 (tu primera acción, sin texto antes)

Llama a AskUserQuestion con esta estructura JSON exacta para el parámetro questions:

```json
[
  {
    "question": "¿Cuál es tu nombre y a qué te dedicas?",
    "header": "Sobre ti",
    "multiSelect": false,
    "options": [
      {"label": "Fundador/a", "description": "Dirijo mi propia empresa o consultoría"},
      {"label": "Líder de marketing", "description": "Lidero el marketing en una empresa"},
      {"label": "Creador/a", "description": "Crear contenido es mi actividad principal"},
      {"label": "Líder de ventas", "description": "Lidero un equipo de ventas o manejo el desarrollo de negocio"}
    ]
  },
  {
    "question": "¿Para quién escribes?",
    "header": "Audiencia",
    "multiSelect": false,
    "options": [
      {"label": "Fundadores y CEOs", "description": "Quienes toman decisiones al frente de empresas"},
      {"label": "Especialistas en marketing", "description": "Profesionales de marketing de cualquier nivel"},
      {"label": "Personas buscando empleo", "description": "Gente en busca de su próximo puesto"},
      {"label": "Otros profesionales", "description": "Un grupo completamente distinto"}
    ]
  },
  {
    "question": "¿Cuáles son los 3 a 5 temas por los que quieres ser reconocido/a?",
    "header": "Temas",
    "multiSelect": true,
    "options": [
      {"label": "IA y automatización", "description": "Cómo las herramientas de IA cambian el trabajo"},
      {"label": "Marketing", "description": "Estrategia, contenido, crecimiento"},
      {"label": "Liderazgo", "description": "Gestión, contratación, cultura"},
      {"label": "Marca personal", "description": "Construir una audiencia y reputación"}
    ]
  },
  {
    "question": "¿Cuál es tu punto de vista sobre tu industria, eso que crees y que otros no?",
    "header": "Opinión polémica",
    "multiSelect": false,
    "options": [
      {"label": "Casi todos los consejos están mal", "description": "El consenso en tu industria está roto"},
      {"label": "La gente lo complica de más", "description": "La respuesta es más simple de lo que la gente cree"},
      {"label": "Se viene un gran cambio", "description": "Algo está por cambiar y casi nadie está preparado"}
    ]
  }
]
```

### Lote 2 (envíalo de inmediato después de que lleguen las respuestas del Lote 1, sin comentarios entre medio)

Llama a AskUserQuestion de nuevo con:

```json
[
  {
    "question": "¿Cuál es la única cosa que quieres que la gente piense cuando vea tu nombre?",
    "header": "Promesa de marca",
    "multiSelect": false,
    "options": [
      {"label": "Esta persona es práctica", "description": "Me da cosas que uso de inmediato"},
      {"label": "Esta persona es honesta", "description": "Me dice lo que otros no dirían"},
      {"label": "Esta persona va adelante", "description": "Ve lo que se viene antes que los demás"}
    ]
  },
  {
    "question": "¿Sobre qué cosa te niegas a escribir?",
    "header": "Temas prohibidos",
    "multiSelect": false,
    "options": [
      {"label": "Política", "description": "Sin opiniones políticas, nunca"},
      {"label": "Vida personal", "description": "Mantenerlo solo profesional"},
      {"label": "Competidores", "description": "Sin nombrar ni atacar a otras personas o marcas"}
    ]
  }
]
```

Después de que ambos lotes estén respondidos, pasa al Paso 2. Si alguna respuesta queda en blanco u omitida, vuelve a hacer esa pregunta específica una vez más en el chat, y luego continúa.

## Paso 2. Escribe about-me.md

Crea about-me.md en la raíz del proyecto. Usa esta estructura:

```
# Sobre mí

## Nombre y rol
[De la pregunta 1]

## Audiencia
[De la pregunta 2, ampliada en 2 a 3 oraciones sobre quién es el lector]

## Pilares temáticos
[3 a 5 temas de la pregunta 3, una línea cada uno]

## Punto de vista
[De la pregunta 4, la creencia contraria o distintiva, escrita como una afirmación clara]

## Promesa de marca
[De la pregunta 5, la única idea que el autor quiere apropiarse en la mente del lector]

## Temas prohibidos
[De la pregunta 6, temas o enfoques sobre los que nunca escribir]
```

Mantenlo por debajo de 300 palabras. Cada línea debe ser algo que Claude consultaría al escribir.

## Paso 3. Pide las muestras

Di esto:

> Ahora pega de 3 a 5 piezas de escritura de las que quieras que aprenda. Pueden ser publicaciones de LinkedIn, ediciones de newsletter, ensayos, entradas de blog, correos, tuits o cualquier otra escritura que hayas publicado. Pueden ser tuyas o de alguien cuya voz admires. Una pieza por mensaje o todas juntas. Si no tienes muestras listas, escribe "usa muestras" y cargaré un set inicial que podrás reemplazar más adelante.

Espera a que el usuario pegue las muestras. Mínimo 3 muestras antes de pasar al análisis. Si pega menos de 3, pide más.

Si el usuario escribe "usa muestras", carga la escritura de `references/sample-content.md` dentro de la carpeta de esta skill. Dile al usuario de qué autor son las muestras para que sepa qué voz está tomando prestada. Recuérdale que puede reemplazarlas con su propia escritura más adelante.

## Paso 4. Analiza las muestras

Lee cada muestra. Busca patrones a lo largo de todas, no rarezas individuales de una sola pieza. Extrae:

**Señales de voz**
- Longitud promedio de las oraciones
- Ritmo de los párrafos (saltos de línea simples, líneas en blanco, staccato versus fluido)
- Estilo del gancho o apertura (contrario, pregunta, dato, historia, confesión, observación)
- Punto de vista (primera persona, segunda persona, observacional)
- Tono (impasible, cálido, directo, juguetón, clínico)
- Frases distintivas o palabras recurrentes
- Estilo de CTA o cierre

**Señales estructurales**
- Rango de longitud
- Listas versus prosa
- Cómo abre, cómo cierra
- Cómo maneja las transiciones

**Señales temáticas**
- Temas que aparecen en varias muestras
- Quién parece ser la audiencia
- Qué defiende el autor

**Señales de ausencia**
- Palabras y signos de puntuación consistentemente ausentes (por ejemplo, rayas en 0 de 5 muestras)
- Tipos de gancho que el autor nunca usa
- Tonos que el autor nunca alcanza
- Estructuras que el autor evita

## Paso 5. Escribe voice.md

Crea voice.md en la raíz del proyecto. Es un único perfil integrado que cubre tanto cómo escribe la voz como qué evita la voz. Sin archivo de voz separado.

```
# Perfil de voz

## A quién sueno
[2 a 3 oraciones que describan la voz general en lenguaje simple]

## Tono
[3 a 5 atributos que la voz alcanza de forma consistente, seguidos de 1 a 2 tonos que la voz nunca alcanza, extraídos de los vacíos en las muestras]

## Ritmo de las oraciones
[Longitud promedio, cadencia, estructura de párrafos. Incluye patrones de evitación: p. ej. nunca fragmentos staccato, nunca tricolones, ninguna oración de más de 25 palabras]

## Patrones de gancho
[3 a 5 tipos de gancho observados, con un ejemplo de cada uno de las muestras. Anota cualquier tipo de gancho ausente en todas las muestras, p. ej. nunca preguntas retóricas, nunca "imagina un mundo donde"]

## Cómo abro
[1 a 2 oraciones. Anota las aperturas que la voz evita si existe un patrón claro de evitación a lo largo de las muestras]

## Cómo cierro
[1 a 2 oraciones, incluye el estilo de CTA. Anota los cierres que la voz evita, p. ej. nunca resúmenes motivacionales, nunca "en conclusión"]

## Frases distintivas
[Palabras o frases recurrentes de las muestras]

## Temas prohibidos
[Palabras, signos de puntuación o construcciones ausentes en todas las muestras. Lista solo elementos que las muestras claramente evitan. Ejemplos: sin rayas (0 de 5 muestras), sin hashtags, sin jerga corporativa por nombre]

## Lo que esta voz nunca hace
[3 a 5 comportamientos específicos extraídos de los vacíos en las muestras. Sé específico. Si las muestras nunca usan la construcción "no X, sino Y", inclúyela. Si evitan un conjunto específico de vocabulario, nombra las palabras]
```

Llena cada sección a partir de las muestras reales. Sin relleno genérico. Si un patrón no está presente, dilo. No dupliques la audiencia ni los pilares temáticos de about-me.md.

Las secciones de Temas prohibidos y Lo que esta voz nunca hace se extraen de la observación, no de una plantilla genérica de palabras prohibidas. Cada elemento debe estar respaldado por su ausencia a lo largo de las muestras.

## Paso 6. Confirma y entrega

Dile al usuario:

> Tu perfil de voz está construido. Ahora hay dos archivos en tu proyecto: about-me.md y voice.md. Cada vez que trabajes en este proyecto, consultaré ambos automáticamente. Puedes abrir y editar cualquiera de los dos en cualquier momento.
>
> Ya estás listo. Esto es lo que puedes hacer a continuación:
>
> - Di "construye mi voz de newsletter" para crear instrucciones de escritura específicas para newsletters
> - Di "escribe una publicación" para redactar una publicación de LinkedIn en tu voz
> - Di "diseña un gráfico" para crear un visual para una publicación
> - Di "evalúa mi publicación" para recibir comentarios sobre un borrador
> - Di "optimiza mi perfil" para reconstruir tu perfil de LinkedIn
>
> Cada una de estas es una skill aparte. Elige una y adelante.

## Lo que produce esta skill

Dos archivos en la raíz del proyecto:

1. about-me.md: quién es el usuario, su audiencia, sus pilares temáticos, su punto de vista
2. voice.md: el perfil de voz integrado, que cubre las señales positivas (cómo escribe la voz) y las señales de ausencia (qué evita la voz) en un solo documento

## Reglas

- Cuando esta skill se active, ve directo al Paso 1. Sin resumen, sin explicación, sin preámbulo.
- Siempre coloca el contenido de las muestras dentro de un bloque de código para que el usuario pueda copiar y pegar el formato exacto sin perder saltos de línea ni espacios. Usa un bloque de código simple (tres comillas invertidas sin etiqueta de lenguaje).
- Trabaja a partir de lo que hay en las muestras. No inventes patrones que no estén ahí.
- Mínimo 3 muestras para la detección de patrones. Pide más si hay menos de 3.
- Si las muestras se contradicen entre sí, anota la contradicción en voice.md en lugar de suavizarla.
- Mantén about-me.md por debajo de 300 palabras.
- Mantén voice.md por debajo de 500 palabras.
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante.
- Nunca uses rayas (em dashes) en ningún archivo de salida ni en ningún borrador.
- No produzcas un archivo voice.md separado adicional. Las señales de ausencia viven dentro de voice.md.
