---
name: publicacion-con-cita
description: >
  Flujo de trabajo de dos pasos para crear publicaciones con cita en LinkedIn. Claude genera citas motivacionales virales para acompañar un texto, y luego produce un prompt de Gemini que recrea una imagen de referencia con la cita elegida ya integrada. Usa esta skill siempre que el usuario diga "publicación con cita", "gráfico con cita", "publicación motivacional", "créame una cita", o quiera un gráfico de LinkedIn de poco esfuerzo y alta interacción. Optimizado para la audiencia de empleados y de inicio de carrera de LinkedIn, que se inclina hacia el contenido motivacional.
---

# Publicación con Cita

## CRÍTICO: Inicio automático al cargar

Cuando esta skill se active, ve directo al Paso 1. No resumas.

## Paso 1. Obtener el texto

Pregunta:

> Pega el texto que esta cita va a acompañar. La cita debe reforzar el mensaje del texto.

Espera el texto.

## Paso 2. Generar opciones de cita

Devuelve 9 opciones de citas motivacionales virales, agrupadas en 3 categorías de 3 citas cada una:

- **Categoría 1: Crecimiento y transformación** (p. ej., "No encuentras el tiempo. Lo creas.")
- **Categoría 2: Resiliencia y temple** (p. ej., "Tu revés es el punto de partida de otra persona.")
- **Categoría 3: Contrario / audaz** (p. ej., "Deja de pedir permiso para empezar.")

Cada cita debe:

- Tener menos de 15 palabras
- Sentirse humana y auténtica, no corporativa
- Evitar jerga o lenguaje demasiado técnico
- Funcionar como una línea independiente sin contexto
- Golpear fuerte en las primeras 3 palabras

Formato de salida:

```
OPCIONES DE CITA para tu texto

1. Crecimiento y transformación
   a. [cita]
   b. [cita]
   c. [cita]

2. Resiliencia y temple
   a. [cita]
   b. [cita]
   c. [cita]

3. Contrario / audaz
   a. [cita]
   b. [cita]
   c. [cita]
```

Luego pregunta:

> ¿Cuál encaja mejor con tu audiencia? Responde con el número y la letra (p. ej. 2b) o pega tu propia cita si ninguna te convence.

## Paso 3. Obtener la imagen de referencia

Una vez que el usuario haya elegido una cita, pregunta:

> Pega o describe la imagen de referencia que quieres recrear. Pinterest, LinkedIn o Google Imágenes sirven. Si no tienes una, te sugeriré un estilo.

Si el usuario describe un estilo en lugar de subir una imagen, recomienda:

- Estilo **cuaderno / dibujado a mano** (boceto simple, fondo crema, marcas de lápiz)
- **Editorial minimalista** (tipografía serif grande, mucho espacio en blanco, un color de acento)
- **Póster audaz** (sans-serif pesada, fondo de bloque de color sólido, alto contraste)
- **Polaroid o foto de película** con texto superpuesto

## Paso 4. Generar el prompt de Gemini

Genera el siguiente prompt en un bloque de código, con la cita ya completada:

```
Recrea la imagen de referencia adjunta con la siguiente cita:

"[CITA ELEGIDA]"

Restricciones críticas:
- Salida a exactamente 1080 x 1350 píxeles (4:5 vertical)
- Coincide con el estilo, la tipografía y la paleta de colores de la imagen de referencia
- Mantén la cita como punto focal: centrada y legible
- No atribuyas nada (sin nombres, sin handles, sin logotipos)
- Mantén el tono visual del original pero con el nuevo texto

La cita debe estar perfectamente escrita y con la puntuación exacta como aparece arriba.
```

Dile al usuario:

> Pega esto en un nuevo chat de Gemini con la imagen de referencia adjunta. Modo Crear Imagen, modelo Nano Banana, salida 1080x1350.

## Paso 5. Establecer expectativas con honestidad

Después del prompt, agrega:

> Las publicaciones con cita logran fuerte interacción pero menos impresiones que otros formatos. No es el tipo de contenido más fuerte. Pero por el esfuerzo, el retorno vale la pena. Esto toma minutos.

## Reglas

- Genera siempre a 1080x1350. Los gráficos de cita horizontales se pierden en el feed de LinkedIn.
- Nunca permitas más de 15 palabras en la cita final. Las citas más largas pierden legibilidad.
- Nunca inventes atribuciones. Las citas se escriben desde cero, no se toman de personas reales a menos que el usuario lo pida.
- Nunca uses guiones largos (em dashes) en ninguna salida.
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante, o salvo que voice.md especifique lo contrario.
- Ajusta las opciones de cita a la voz del usuario si existe voice.md.
- Si la voz del usuario explícitamente no es motivacional (analítica, solo contraria, seca), señala el desajuste y pregunta si las publicaciones con cita encajan con su posicionamiento antes de generar.
