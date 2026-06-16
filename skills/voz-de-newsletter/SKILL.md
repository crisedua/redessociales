---
name: voz-de-newsletter
description: >
  Construye instrucciones de escritura para newsletters dentro de un proyecto de Cowork. Se ejecuta después de constructor-de-voz. Produce voz-de-newsletter.md, un único archivo que Claude consulta al redactar newsletters en la voz del usuario. Funciona con o sin muestras de newsletters previas: si el usuario tiene ediciones pasadas, la skill las analiza; si no, la skill ofrece 6 arquetipos ajustados a la voz del usuario. Actívala cada vez que el usuario diga "construye mi voz de newsletter", "aprende mi estilo de newsletter", "configura mi sistema de newsletter", "aprende de mis newsletters", "onboarding de newsletter", o cuando deje muestras de newsletter en el chat pidiendo un análisis. Requiere que constructor-de-voz se haya ejecutado primero: la skill necesita voice.md y about-me.md en el proyecto para funcionar.
---

# Newsletter Voice

## Verificación de prerrequisitos

En el momento en que esta skill se active, revisa la raíz del proyecto en busca de voice.md y about-me.md.

Si falta cualquiera de los dos archivos, dile al usuario:

> La voz de newsletter se apoya sobre tu perfil de voz general. Ejecuta constructor-de-voz primero (sube la skill o di "construye mi voz") y luego regresa aquí una vez que about-me.md y voice.md estén en el proyecto.

Luego detente. No continúes hasta que ambos archivos existan.

Si ambos archivos existen, léelos por completo y luego ve directo al Paso 1.

## Paso 1. Revisa si hay muestras

Pregunta al usuario en el chat:

> ¿Tienes de 2 a 3 ediciones anteriores de newsletter de las que pueda aprender?
>
> Sí: pégalas aquí (una por mensaje o todas juntas)
> No: escribe "arquetipo" y construiré a partir de una plantilla ajustada a tu voz

Espera la respuesta.

Si el usuario pega 2 o más newsletters, ve al Paso 2a.
Si el usuario escribe "arquetipo", ve al Paso 2b.
Si el usuario pega 1 newsletter, pide al menos una más. Si solo tiene una, ofrece: "Una sola muestra no es suficiente para la detección de patrones. ¿Quieres que cambie a modo arquetipo y use tu única newsletter como punto de referencia?"

## Paso 2a. Análisis basado en muestras

Lee cada newsletter por completo. Busca patrones a lo largo de las ediciones, no rarezas aisladas. Extrae:

**Fórmula de apertura**
- Qué hacen las primeras 3 oraciones (resultado específico, observación cultural, afirmación, escena, pregunta)
- Longitud de la sección de apertura antes del primer corte estructural
- Movimiento de credibilidad (cómo el autor establece autoridad temprano)
- Promesa de valor (qué se le dice al lector que obtendrá)

**Estructura de secciones**
- Planteamiento de problema o contraste
- Marco con nombre o prosa libre
- Pasos numerados, métodos o argumento continuo
- Patrones de ejemplos y evidencia
- Sección de bonus o extensión
- Fórmula de cierre y despedida

**Filosofía de datos**
- Números específicos por edición (cuéntalos)
- Estilo de atribución de fuentes (enlazadas, nombradas, sin crédito)
- Proporción de ejemplo a abstracción
- Reconocimientos de limitaciones o fracasos

**Formato**
- Uso de encabezados (frecuencia, jerarquía)
- Uso de listas (numeradas, con viñetas, con flechas)
- Uso de negrita y cursiva
- Formato de prompts, bloques de código o citas en bloque
- Marcadores visuales (flechas, marcas de verificación, emojis si los hay)

**Longitud**
- Rango de número de palabras a lo largo de las muestras
- Número de palabras por sección

**Marcadores de voz propios del formato newsletter**
- Pro tips o llamados de atención (frecuencia, formato)
- Cierres con mirada hacia adelante
- Frase de despedida si es consistente a lo largo de las muestras
- Meta-transparencia (¿el autor reflexiona sobre el proceso o pide retroalimentación?)

**Señales de ausencia**
- Palabras, construcciones o estructuras ausentes en todas las muestras
- Cierres que el autor nunca usa
- Temas que el autor nunca toca

Luego ve al Paso 3.

## Paso 2b. Selección de arquetipo

Llama a la herramienta AskUserQuestion con una sola pregunta:

```json
[
  {
    "question": "¿Qué arquetipo de newsletter encaja con lo que quieres escribir?",
    "header": "Arquetipo",
    "multiSelect": false,
    "options": [
      {"label": "Tutorial con datos", "description": "Números, marcos y métodos paso a paso con prompts"},
      {"label": "Ensayo contrario", "description": "Toma una postura, defiéndela, nombra a la oposición"},
      {"label": "Análisis de caso", "description": "Un tema por edición, desmenuzado en profundidad"},
      {"label": "Resumen curado", "description": "De 5 a 7 enlaces con tu comentario cada semana"},
      {"label": "Ensayo personal", "description": "Reflexión sobre un tema, la historia primero"},
      {"label": "Entrevista o perfil", "description": "Una persona por edición, formato pregunta y respuesta o narrativo"}
    ]
  }
]
```

Después de que el usuario elija un arquetipo, carga los valores predeterminados correspondientes de `references/archetypes.md` dentro de la carpeta de esta skill. Ajusta cada campo usando voice.md y about-me.md antes de escribir voz-de-newsletter.md. Señala dentro del archivo de salida que se usaron los valores predeterminados del arquetipo y que el archivo debe revisarse después de 5 ediciones publicadas.

## Paso 3. Escribe voz-de-newsletter.md

Crea voz-de-newsletter.md en la raíz del proyecto. Archivo único, objetivo de 800 a 1.200 palabras. Usa esta estructura:

```
# Voz de newsletter

## Origen
[Basado en muestras: se analizaron X ediciones de newsletter] O [Basado en arquetipo: [nombre del arquetipo] ajustado a voice.md. Revisar después de 5 ediciones publicadas.]

## Audiencia y propósito
[Quién lee esta newsletter y qué obtiene de ella. Escrito a partir de about-me.md y voice.md. 2 a 3 oraciones.]

## Principios de voz
[3 a 5 principios fundamentales que la escritura siempre mantiene. Cada uno una oración declarativa breve. Ajustados a la voice.md de este usuario.]

## Fórmula de apertura
[Cómo empiezan las ediciones. Incluye 2 plantillas concretas con marcadores entre corchetes, p. ej. "[Resultado específico con número]. [Marcador de credibilidad]. [Promesa de valor para esta edición]." Objetivo de número de palabras para la sección de apertura.]

## Flujo de secciones
[Estructura estándar de una edición, sección por sección, máximo 5 a 8 secciones. Notas breves sobre qué hace cada sección y cuánto se extiende.]

## Datos y evidencia
[Cómo se usan los números y ejemplos. Reglas específicas, p. ej. "cada afirmación necesita un número", "fuentes enlazadas en línea", "proporción de ejemplo a abstracción de aproximadamente 3:1".]

## Reglas de formato
[Encabezados, listas, negrita, cursiva, bloques de código, marcadores visuales. Qué usar, qué evitar. Extraído de las muestras o de los valores predeterminados del arquetipo.]

## Cierre y despedida
[Cómo terminan las ediciones. Afirmación con mirada hacia adelante versus resumen. Frase de despedida si existe una consistente a lo largo de las muestras (no inventes una).]

## Lo que esta newsletter nunca hace
[1 párrafo breve o 3 a 5 elementos. Extraído de patrones de ausencia a lo largo de las muestras o de los valores predeterminados del arquetipo. Solo comportamientos, no una lista de palabras prohibidas.]

## Longitud
[Objetivo de número de palabras para ediciones estándar. Objetivo aparte para guías completas más largas si el usuario escribe ambos formatos.]
```

Llena cada sección a partir de las muestras (o de los valores predeterminados del arquetipo ajustados). Sin relleno genérico. Si las muestras no cubren algo, di "no hay un patrón claro a lo largo de las muestras" en lugar de adivinar.

## Paso 4. Confirma y entrega

Dile al usuario:

> Tu voz de newsletter está construida. voz-de-newsletter.md está en la raíz de tu proyecto junto a about-me.md y voice.md. Cuando quieras redactar una edición, di "escribe una newsletter" y usaré los tres archivos en conjunto.
>
> [Si es modo arquetipo: Recuerda que este archivo se construyó a partir de valores predeterminados de arquetipo. Después de unas 5 ediciones publicadas, vuelve a ejecutar esta skill con tus muestras reales para obtener un perfil más afilado.]

## Lo que produce esta skill

Un archivo en la raíz del proyecto:

- voz-de-newsletter.md: instrucciones de escritura específicas para newsletters que cubren audiencia, principios de voz, fórmula de apertura, flujo de secciones, filosofía de datos, formato, cierre, patrones de ausencia y objetivos de longitud

## Reglas

- Requiere voice.md y about-me.md en la raíz del proyecto antes de ejecutarse. Detente y redirige a constructor-de-voz si falta cualquiera de los dos.
- Mínimo 2 muestras de newsletter si el usuario elige el modo basado en muestras. Ofrece el modo arquetipo si hay menos.
- Mantén voz-de-newsletter.md por debajo de 1.200 palabras. Conciso le gana a exhaustivo.
- No inventes señales de voz. Trabaja solo a partir de muestras o de valores predeterminados de arquetipo ajustados a voice.md.
- No dupliques contenido de voice.md. Refiérete a él donde sea relevante. voz-de-newsletter.md solo agrega reglas específicas para newsletters.
- No incorpores los nombres, URLs o frases de despedida específicos del usuario a menos que aparezcan de forma consistente en 2 o más muestras.
- No produzcas un archivo separado de voz o de palabras prohibidas. Los patrones de ausencia viven dentro de voz-de-newsletter.md como una sola sección.
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante.
- Nunca uses rayas (em dashes) en ningún archivo de salida ni en ningún borrador.
