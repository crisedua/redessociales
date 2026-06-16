---
name: evaluador-de-publicaciones
description: >
  Califica una publicación de LinkedIn usando datos reales de rendimiento. Obtiene el historial de publicaciones del propio usuario mediante Apify (o usa datos en caché) para identificar lo que realmente funciona, y luego califica el borrador contra esos patrones. Usa esta skill cada vez que el usuario diga "califica mi publicación", "revisa mi publicación", "puntúa esta publicación", "dame feedback", "qué tan buena es esta publicación", o pegue una publicación de LinkedIn y pida una crítica. Califica contra datos reales, no contra consejos genéricos. Diseñada para calificación en vivo en eventos y para la revisión diaria de publicaciones.
---

# Post Scorer

## IMPORTANTE: Inicio automático al cargar

Cuando esta skill se active, ve directamente al Paso 1. No resumas. No expliques el método de calificación. Comienza de inmediato.

## Paso 1. Obtén la publicación

Si el usuario ya pegó una publicación en el mismo mensaje, úsala. De lo contrario, di:

> Pega la publicación de LinkedIn que quieres calificar.

Espera la publicación.

## Paso 2. Carga los datos de calificación

El calificador necesita dos cosas: el sistema de voz del usuario y datos reales de rendimiento.

### Sistema de voz

Lee about-me.md y voice.md del proyecto si existen. Si faltan, anótalo y califica sin emparejamiento de voz.

### Datos de rendimiento

Busca datos de LinkedIn en caché en el proyecto o en la carpeta de salidas. Busca archivos que coincidan con *-all-posts.json o *-posts.txt.

Si existen datos en caché, úsalos. Si no, pregunta al usuario:

```json
[
  {
    "question": "Para calificar tu publicación contra datos reales, necesito tu historial de LinkedIn. ¿Cómo debo obtenerlo?",
    "header": "Fuente de datos",
    "multiSelect": false,
    "options": [
      {"label": "Extraer mis publicaciones", "description": "Obtener mis últimas 100 publicaciones de LinkedIn vía Apify. Tarda de 1 a 2 minutos y cuesta alrededor de $0.50."},
      {"label": "Usar datos de Charlie Hills", "description": "Calificar contra los benchmarks de Charlie Hills (1.872 de engagement promedio, 500 publicaciones analizadas). Buena alternativa."},
      {"label": "Omitir la calificación con datos", "description": "Calificar solo contra buenas prácticas genéricas. Menos preciso pero instantáneo."}
    ]
  }
]
```

Si elige "Extraer mis publicaciones":
1. Pide su nombre de usuario de LinkedIn
2. Llama al actor de Apify apimaestro/linkedin-profile-posts con la entrada: { "username": "[su-username]", "total_posts": 100 }
3. Descarga los resultados (NO uses el parámetro fields, elimina los datos de engagement)
4. Guarda como [username]-all-posts.json en el proyecto
5. Continúa al análisis

Si elige "Usar datos de Charlie Hills":
Busca los datos de Charlie en caché en **/linkedin-data/charlie-all-posts.json. Si los encuentras, úsalos. Si no, anota que estás usando los benchmarks de este archivo de skill (listados abajo).

Si elige "Omitir la calificación con datos":
Recurre a la calificación basada solo en el sistema de voz y en buenas prácticas generales.

## Paso 3. Analiza las publicaciones de mejor rendimiento

Cuando haya datos de rendimiento disponibles, ejecuta este análisis antes de calificar:

1. Calcula el puntaje de engagement de cada publicación: total_reactions + (comments x 3)
2. Identifica el 10% superior de publicaciones por puntaje de engagement
3. De esas publicaciones top, extrae:
   - Los tipos de gancho que aparecen con más frecuencia (contrario, liderado por número, afirmación contundente, historia personal, pregunta, noticia)
   - Longitud promedio de la publicación (conteo de palabras)
   - Distribución de formatos (solo texto, imagen, carrusel, video)
   - Patrones de CTA (mención de newsletter, comment gate, pedido de compartir, pregunta, ninguno)
   - Grupos de temas que sobresalen en engagement
   - Ritmo de las oraciones (longitud promedio de oración, saltos de párrafo por publicación)
4. Anota también los patrones del 10% inferior para identificar qué falla

Guarda estos patrones como un "perfil de calificación" que consultes para cada criterio.

## Paso 4. Califica la publicación

Califica con base en 5 criterios. Cada uno se puntúa de 1 a 10.

### Fuerza del gancho (1 a 10)

Compara la línea de apertura del borrador con los tipos de gancho del 10% superior.
- ¿Usa un tipo de gancho que históricamente funciona para este autor?
- ¿Es específico, con un número, un nombre o un detalle concreto?
- ¿Detendría el scroll según lo que realmente detiene el scroll en sus datos?
- Califica 8+ solo si el tipo de gancho coincide con un patrón de su 10% superior

### Coincidencia de voz (1 a 10)

Si existe voice.md:
- ¿La publicación coincide con el tono, el ritmo y la longitud de oración de voice.md?
- ¿Viola alguna regla de la sección de patrones de ausencia de voice.md (lo que la voz nunca hace)?
- ¿La longitud de las oraciones coincide con el promedio de sus publicaciones de mejor rendimiento?
Si no hay archivos de voz: califica contra los patrones extraídos de los datos de sus publicaciones.

### Densidad de valor (1 a 10)

Compara con las publicaciones de mejor rendimiento del usuario:
- ¿Sus mejores publicaciones enseñan, dan pasos, comparten datos o cuentan historias?
- ¿Este borrador coincide con ese patrón de valor?
- ¿La conclusión es lo bastante específica como para que alguien la guarde o la comparta?
- Compara el conteo de palabras con el promedio de su 10% superior. Marca si está muy por encima o por debajo.

### Estructura y formato (1 a 10)

Con base en sus datos:
- ¿Qué formato (texto, imagen, carrusel) obtiene más engagement para él/ella?
- ¿La estructura del borrador coincide con el ritmo de saltos de línea y párrafos de las publicaciones top?
- ¿La publicación es fácil de leer en móvil?
- ¿El CTA coincide con los patrones de sus publicaciones de mejor rendimiento?

### Listo para publicar (1 a 10)

- ¿El usuario realmente escribió esto o se lee como salida de IA sin editar?
- ¿Esta publicación se integraría de forma natural en su feed según su historial de publicaciones?
- ¿Hay señales de alerta: palabras prohibidas listadas en la sección de patrones de ausencia de voice.md, frases genéricas, tono corporativo?
- ¿Tiene la longitud adecuada en comparación con sus publicaciones de mejor rendimiento?

## Paso 5. Entrega la tarjeta de calificación

Entrega dentro de un bloque de código:

```
PUNTAJE DE PUBLICACIÓN DE LINKEDIN

Fuente de datos: [tus publicaciones / benchmarks de Charlie Hills / genérico]
Publicaciones analizadas: [número]
Engagement promedio del 10% superior: [número]

Fuerza del gancho:      [X] / 10  [tipo de gancho detectado]
Coincidencia de voz:    [X] / 10
Densidad de valor:      [X] / 10
Estructura y formato:   [X] / 10  [formato: texto/imagen/carrusel]
Listo para publicar:    [X] / 10
----------------------------------------
TOTAL:                  [XX] / 50

VEREDICTO: [Una oración que haga referencia a datos específicos]

COMPARACIÓN CON LAS PUBLICACIONES DE MEJOR RENDIMIENTO:
Tus mejores publicaciones promedian [X] palabras, usan ganchos de tipo [tipo de gancho],
e incluyen [patrón de CTA]. Este borrador [coincide/difiere] porque [razón específica].

ARREGLOS:
1. [Arreglo específico respaldado por datos, p. ej. "Tus publicaciones del 10% superior abren con números. Esta abre con una pregunta. Cámbialo a una estadística."]
2. [Segundo arreglo respaldado por datos]
3. [Tercer arreglo si hace falta]
```

Cada arreglo debe hacer referencia a los datos reales del usuario. No "mejora el gancho", sino "tus publicaciones del 10% superior usan ganchos liderados por número (42% de los aciertos). Este borrador usa un gancho de pregunta (12% de los aciertos). Mejor lidera con la estadística."

## Paso 6. Ofrece los siguientes pasos

Después de la tarjeta de calificación:

> ¿Quieres que reescriba la sección más débil usando patrones de tus mejores publicaciones, o la publicas tal cual?

Si se solicita la reescritura, aplica los arreglos y entrega la publicación revisada en un bloque de código.

## Benchmarks de respaldo (cuando no hay datos disponibles)

Usa estos benchmarks de Charlie Hills como base de calificación cuando el usuario elija "Usar datos de Charlie Hills" y no se encuentre un archivo en caché:

Engagement promedio: 1.872 (reacciones + comentarios x 3)
Reacciones promedio: 808
Comentarios promedio: 355
Compartidos promedio: 61
Ratio comentarios-reacciones: 44%

Tipos de gancho principales: liderado por número (31%), afirmación contundente (27%), contrario (18%)
Formatos principales: carrusel (33%), imagen (29%), solo texto (22%)
Longitud promedio de publicación del 10% superior: de 180 a 250 palabras
Tasa de CTA: 45% mencionan newsletter
Tasa de comment gate: 5%

## Reglas

- Siempre intenta usar datos reales antes de recurrir a consejos genéricos.
- Cada puntaje y cada arreglo debe hacer referencia a puntos de datos específicos, no a opiniones subjetivas.
- Nunca califiques por encima de 8 a menos que el borrador realmente coincida con los patrones del 10% superior.
- Sé honesto. Un calificador generoso es inútil.
- Si los datos están desactualizados (14+ días de antigüedad), sugiere una actualización antes de calificar.
- Informa al usuario antes de ejecutar una extracción con Apify (cuesta dinero).
- Nunca uses rayas (em dash) en ningún resultado.
- Español (Latinoamérica) en todo momento.
- Mantén la tarjeta de calificación compacta. Tiene que verse bien en una pantalla grande en eventos.
