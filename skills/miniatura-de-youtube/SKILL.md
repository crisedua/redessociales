---
name: miniatura-de-youtube
description: >
  Genera una miniatura de YouTube con tu marca a partir del título de un video. Usa una foto de referencia del creador, principios de miniaturas de alto CTR y colores de marca para producir un prompt de imagen listo para generar en Gemini. Usa esta skill siempre que el usuario diga "miniatura", "miniatura de youtube", "hazme una miniatura", o quiera una imagen de portada de video antes de escribir el guion. El flujo de trabajo de miniatura primero refleja el enfoque de gráfico primero para LinkedIn: vende el video antes de que nadie escuche una palabra del guion.
---

# YouTube Thumbnail

## CRÍTICO: Inicio automático al cargar

Cuando esta skill se active, ve directo al Paso 1.

## Paso 1. Reúne los insumos

Revisa el proyecto en busca de una configuración de foto de referencia. Busca en este orden:

1. `thumbnail-config.md` en la raíz del proyecto
2. `brand-kit.md` — busca una ruta de imagen de referencia y colores de marca
3. `about-me.md` — para el nombre y el posicionamiento del creador

Si hay una ruta de foto de referencia guardada, pre-cárgala. Si no, pregunta:

> Sube o indica la ruta de la foto de referencia tuya que quieres usar en la miniatura. Idealmente un retrato claro con iluminación y expresión distintivas que planees reutilizar en todos tus videos para consistencia de marca.

Luego llama a AskUserQuestion:

```json
[
  {
    "question": "¿Cuál es el título del video?",
    "header": "Título",
    "multiSelect": false,
    "options": [
      {"label": "Yo escribiré el título", "description": "Escribe el título de trabajo completo"},
      {"label": "Sugiéreme uno", "description": "Dado el tema, propón primero 3 títulos atractivos para hacer clic"}
    ]
  },
  {
    "question": "¿Tono emocional?",
    "header": "Tono",
    "multiSelect": false,
    "options": [
      {"label": "Shock / sorpresa", "description": "Ojos muy abiertos, boca abierta, reacción marcada"},
      {"label": "Curioso / pensativo", "description": "Leve sonrisa de lado, ceja levantada, mirada fuera de cuadro"},
      {"label": "Seguro / directo", "description": "Contacto visual, calma, actitud firme"},
      {"label": "Frustrado / postura fuerte", "description": "Mirada intensa, gesto con la mano, tensión"}
    ]
  }
]
```

## Paso 2. Aplica las mejores prácticas de miniaturas

Toda miniatura debe seguir estas reglas:

- **El rostro ocupa del 30 al 50 por ciento** del cuadro. Legible en tamaños pequeños.
- **Máximo 3 a 5 palabras** de texto grande. 6 si es absolutamente necesario.
- **Dominan dos colores**. El primario de marca + un acento de alto contraste (amarillo, rojo, cian funcionan bien).
- **Un elemento focal claro** además del rostro. Logo de una herramienta, número grande, flecha o accesorio.
- **Alto contraste** entre rostro, texto y fondo. Pruébalo entrecerrando los ojos.
- **El texto no es una oración**. Es una frase gancho. Ejemplos: "Despedí a mi equipo", "Claude ya puede...", "No hagas esto".
- **Sin texto pequeño, sin logos abajo a la derecha** (ahí va el ícono de tiempo de reproducción).

## Paso 3. Arma el brief de la miniatura

Entrega un brief conciso que el usuario pueda revisar:

```
BRIEF DE MINIATURA: [título del video]

Composición: [posición del rostro, % del cuadro, dirección de la mirada]
Texto: "[frase gancho, 3-5 palabras]"
Ubicación del texto: [izquierda, derecha, arriba, rodea el rostro]
Paleta de colores: [hex primario], [hex de acento], [hex de fondo]
Elemento de apoyo: [logo / accesorio / flecha / número]
Tono emocional: [tono del Paso 1]
```

Luego pregunta:

> Aquí está el brief. Di "generar" para entregar el prompt de imagen o dime qué cambiar.

## Paso 4. Entrega el prompt de Gemini

Una vez aprobado, entrega el prompt de generación de imágenes en un bloque de código:

```
Usando la foto de referencia mía adjunta, genera una miniatura de YouTube de 1280 x 720 píxeles (16:9).

Composición:
- Ubícame [izquierda / derecha / centro] ocupando del [30-50]% del cuadro
- Mi expresión: [detalles del tono — p. ej., en shock con ojos muy abiertos y boca abierta]
- Mi mirada: [dirección — p. ej., mirando directo a la cámara / mirando fuera de cuadro hacia el texto]

Texto:
- Muestra "[frase gancho]" en tipografía sans-serif grande y en negrita
- Color del texto: [hex]
- Contorno del texto: [color, grosor para legibilidad]
- Ubicación del texto: [área específica]

Paleta de colores:
- Primario: [hex]
- Acento: [hex]
- Fondo: [hex] — [describe el tratamiento: plano, degradado, escena desenfocada, etc.]

Elemento de apoyo: [descripción específica del visual de apoyo]

Restricciones:
- El rostro debe estar nítido y enfocado
- El texto debe ser legible a 320px de ancho (tamaño móvil de YouTube)
- Sin marcas de agua, sin elementos de la interfaz de YouTube, sin texto en la esquina inferior derecha
- Alto contraste entre rostro, texto y fondo
```

Dile al usuario:

> Pega esto en un chat nuevo de Gemini, adjunta tu foto de referencia, activa Create Image y selecciona Nano Banana. Genera en 1280x720.

## Paso 5. Ofrece el siguiente movimiento

> ¿Quieres que esboce el video a continuación? Gancho, parte media y CTA a partir de la miniatura. O llama a la skill de creación si tienes una.

## Reglas

- 1280x720 píxeles (16:9). El tamaño nativo de miniatura de YouTube.
- Nunca incluyas la ruta de la foto de referencia en el prompt mismo — el usuario adjunta la foto por separado.
- Nunca permitas más de 6 palabras de texto, 5 es lo ideal, 3 es lo mejor.
- El rostro siempre debe ser un punto focal visible. Sin composiciones con el rostro oculto.
- Nunca uses guiones largos (em dashes).
- Español neutro a menos que voice.md indique lo contrario.
- Si brand-kit.md está en el proyecto, léelo y usa los colores de marca exactos.
- Recomienda al usuario mantener un estilo de miniatura consistente entre videos para el reconocimiento del canal.
