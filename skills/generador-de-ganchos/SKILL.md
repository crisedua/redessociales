---
name: generador-de-ganchos
description: >
  Genera 6 variaciones de ganchos estilo clickbait para LinkedIn sobre cualquier tema. Ganchos de dos líneas construidos sobre la fórmula: una línea de apertura de 40 caracteres y una línea de contraste en negrita de 40 caracteres. Incluye dígitos, frases con "Cómo" o "Yo", y métricas. Usa esta skill cada vez que el usuario diga "escríbeme ganchos", "ideas de ganchos", "genera ganchos", "necesito un gancho para una publicación sobre...", o pegue un tema y pida aperturas. Salida rápida, sin preámbulo.
---

# Hook Generator

## IMPORTANTE: Inicio automático al cargar

Cuando esta skill se active, ve directamente al Paso 1. No resumas. No expliques qué hace que un gancho sea bueno.

## Paso 1. Obtén el tema

Si el usuario ya pegó un tema en su mensaje, úsalo y salta al Paso 2.

De lo contrario, pregunta:

> ¿Para qué tema quieres ganchos?

Espera la respuesta.

## Paso 2. Escribe 6 variaciones de ganchos

Todos los ganchos tienen la misma estructura:

- **Línea 1 (Apertura)**: 40 caracteres como máximo. Sin preguntas. Plantea algo inesperado, específico o contundente.
- **Línea 2 (Contraste)**: 40 caracteres como máximo. Contradice, reformula o desmonta la apertura.

Cada variación debe:

- Incluir al menos una frase con "Cómo" o "Yo" entre las dos líneas
- Incluir un dígito o una métrica donde sea posible
- Seguir los principios del clickbait: tensión, brecha de curiosidad, lo que está en juego

Produce 6 variaciones que cubran ángulos diferentes:

1. **Liderada por número**: Comienza con un número o métrica específicos
2. **Contraria**: Plantea una creencia y luego dale la vuelta
3. **Transformación personal**: Antes vs después con un dígito
4. **Robo de autoridad**: Menciona un nombre, herramienta o marca
5. **Confesión**: Confiesa un error o una pérdida
6. **Choque de futuro**: Una predicción o "X está por cambiar"

## Paso 3. Formato de salida

```
GANCHOS para [tema]

1. [Liderada por número]
[Línea 1]
[Línea 2]

2. [Contraria]
[Línea 1]
[Línea 2]

3. [Transformación personal]
[Línea 1]
[Línea 2]

4. [Robo de autoridad]
[Línea 1]
[Línea 2]

5. [Confesión]
[Línea 1]
[Línea 2]

6. [Choque de futuro]
[Línea 1]
[Línea 2]
```

## Paso 4. Ofrece el siguiente paso

Pregunta:

> ¿Quieres que convierta uno de estos en una publicación completa? Llama a la skill formateador-de-publicaciones con el número del gancho.

## Reglas

- 40 caracteres como máximo por línea. Cuéntalos.
- Sin preguntas en la línea de apertura.
- Sin rayas (em dash).
- Sin palabras de relleno. Cada palabra se gana su lugar.
- Prefiere dígitos en lugar de números escritos (3, no tres).
- Español (Latinoamérica) en todo momento, salvo que voice.md indique lo contrario.
- Nunca titubees. Un gancho débil es peor que ningún gancho.
