---
name: optimizador-de-perfil
description: >
  Reconstruye un perfil de LinkedIn para maximizar las conversiones. Genera nuevas opciones de titular, sección "Acerca de", sección de experiencia, estrategia para la sección destacada y 4 prompts de generación de imágenes (banner, foto de perfil, 2 tarjetas destacadas). Usa esta skill cuando el usuario diga "optimiza mi perfil", "arregla mi LinkedIn", "reescribe mi titular", "revisión de perfil", "auditoría de LinkedIn", "reconstruye mi perfil" o quiera ayuda con cualquier parte de su perfil de LinkedIn. Actívala también cuando el usuario suba un PDF o captura de pantalla de su perfil de LinkedIn para revisarlo.
---

# Profile Optimizer

## CRÍTICO: Inicio automático al cargar

Cuando se active esta skill, ve directo al Paso 1. No resumas. No expliques qué vas a producir. Comienza a recopilar la información de inmediato.

## Paso 1. Recopila la información

Revisa el proyecto en busca de about-me.md. Si existe, completa por adelantado el nombre, la audiencia, los temas y el punto de vista a partir de él. Omite esas preguntas y dile al usuario qué tomaste.

Llama a AskUserQuestion en dos tandas.

### Tanda 1

```json
[
  {
    "question": "¿Cuál es el objetivo principal de tu presencia en LinkedIn?",
    "header": "Objetivo",
    "multiSelect": false,
    "options": [
      {"label": "Llamadas agendadas", "description": "Generar llamadas de descubrimiento o demos"},
      {"label": "Leads entrantes", "description": "Atraer prospectos que te contacten"},
      {"label": "Suscriptores al newsletter", "description": "Hacer crecer tu lista de correo desde LinkedIn"},
      {"label": "Oportunidades laborales", "description": "Lograr que reclutadores y responsables de contratación te contacten"}
    ]
  },
  {
    "question": "¿Cuál es tu oferta o servicio principal?",
    "header": "Oferta",
    "multiSelect": false,
    "options": [
      {"label": "Coaching", "description": "Coaching individual o grupal"},
      {"label": "Consultoría", "description": "Trabajo de asesoría o estrategia"},
      {"label": "Servicios de agencia", "description": "Servicios llave en mano para clientes"},
      {"label": "Freelance", "description": "Trabajo por proyecto o por contrato"}
    ]
  },
  {
    "question": "¿Tienes colores de marca (códigos hex)?",
    "header": "Colores",
    "multiSelect": false,
    "options": [
      {"label": "No, sugiéreme", "description": "Elige colores según mi posicionamiento"},
      {"label": "Sí, los voy a pegar", "description": "Tengo códigos hex específicos para usar"}
    ]
  },
  {
    "question": "¿Hay alguna prueba social que quieras destacar?",
    "header": "Prueba",
    "multiSelect": false,
    "options": [
      {"label": "Años de experiencia", "description": "p. ej. más de 10 años en marketing"},
      {"label": "Resultados de clientes", "description": "p. ej. Ayudé a más de 200 clientes"},
      {"label": "Apariciones en medios", "description": "p. ej. Destacado en Forbes"},
      {"label": "Voy a escribir la mía", "description": "Déjame pegar puntos de prueba específicos"}
    ]
  }
]
```

### Tanda 2

```json
[
  {
    "question": "¿Qué enlaces externos quieres en tu sección destacada? (máximo 2)",
    "header": "Enlaces",
    "multiSelect": false,
    "options": [
      {"label": "Página de reservas + newsletter", "description": "Enlace principal para agendar llamadas, secundario para suscribirse"},
      {"label": "Portafolio + página de reservas", "description": "Muestra el trabajo primero y luego convierte"},
      {"label": "Voy a pegar las URLs", "description": "Tengo enlaces específicos para usar"}
    ]
  },
  {
    "question": "¿Cómo debo obtener tu perfil actual?",
    "header": "Perfil actual",
    "multiSelect": false,
    "options": [
      {"label": "Lo voy a pegar", "description": "Voy a copiar y pegar mi titular, mi sección Acerca de y mi experiencia"},
      {"label": "Empezar de cero", "description": "Ignora mi perfil actual, constrúyelo desde cero usando mi about-me.md"},
      {"label": "Voy a subir una captura", "description": "Voy a compartir una captura de pantalla o un PDF de mi perfil"}
    ]
  }
]
```

Espera a tener toda la información antes de continuar.

## Paso 2. El titular

Escribe 3 opciones de titular. Muéstralas en un bloque de código.

Restricciones:
- Máximo 50 caracteres cada uno
- Solo mayúscula inicial de la oración (no Mayúscula En Cada Palabra)
- Empieza con el valor central
- Incluye la audiencia objetivo cuando el número de caracteres lo permita
- Sin cargos (sin "Fundador" / "CEO" / "Diseñador")
- Sin palabras de relleno

Formato: [Valor central] + [para la audiencia objetivo]

```
Opción 1 (Directa): [resultado + audiencia]
Opción 2 (Enfocada en el dolor): [problema + audiencia]
Opción 3 (Diferenciador): [ángulo único + audiencia]
```

Luego llama a AskUserQuestion para que el usuario elija:

```json
[
  {
    "question": "¿Con qué titular quieres quedarte?",
    "header": "Titular",
    "multiSelect": false,
    "options": [
      {"label": "Opción 1", "description": "[El texto real del titular de arriba]"},
      {"label": "Opción 2", "description": "[El texto real del titular de arriba]"},
      {"label": "Opción 3", "description": "[El texto real del titular de arriba]"}
    ]
  }
]
```

## Paso 3. La sección Acerca de

Muéstrala en un bloque de código. Reglas de formato:
- Escribe en oraciones completas. No fuerces saltos de línea a mitad de una oración.
- Un salto de línea entre oraciones o frases cortas para facilitar la lectura.
- Doble salto de línea entre secciones (gancho, historia, autoridad, CTA).
- LinkedIn gestiona el ajuste de texto en móvil y escritorio. No ajustes manualmente a 50 o 60 caracteres.
- Las oraciones cortas y contundentes son buenas. Cortar oraciones por la mitad no lo es.

Estructura: Gancho > Lucha/Empatía > Método/Filosofía > Autoridad > CTA

Tono: Contundente, directo, humano. No corporativo.

Ejemplo de formato correcto:
```
Dejé mi trabajo de 9 a 5 en septiembre de 2024.
Dos años después: más de 200 mil seguidores, varios negocios de seis cifras, miles de profesionales del marketing formados.

Esto es lo que aprendí.
La mayoría de la gente no necesita más contenido. Necesita un sistema que convierta el contenido en clientes. Eso es lo que construyo.

✦ Linked Agency: contenido de LinkedIn llave en mano para fundadores y ejecutivos.
✦ Vislo: una herramienta de diseño con IA que crea infografías listas para publicar en minutos.
✦ Newsletter MarTech AI: más de 200 mil lectores que reciben frameworks de marketing con IA cada semana.
✦ The AI Creators Club: análisis en vivo, sistemas probados y una comunidad que te ayuda a crecer en semanas, no en meses.

Colaboraciones de marca y conferencias: hello@influencermoso.com

Mira mi sección destacada más abajo. O escríbeme por mensaje directo. Te orientaré en la dirección correcta.
```

## Paso 4. La sección de experiencia

Reescribe los 2 cargos principales. Muéstralos en un bloque de código. Mismo enfoque de formato que la sección Acerca de: oraciones completas, sin saltos de línea forzados a mitad de oración, LinkedIn gestiona el ajuste.

Estructura por cargo: Contexto > Desafío > Acción > Resultado

Formato de storytelling, no viñetas. Máximo de 8 a 15 oraciones por cargo.

Ejemplo de formato correcto:
```
Llegué a un equipo que había perdido 3 gerentes en 2 años.
La moral estaba por el suelo. Los ingresos caían.

No empecé con presentaciones de estrategia. Empecé escuchando.

En 6 meses reconstruimos la cultura.
En 12 meses los ingresos subieron 40%.

Eso me enseñó todo sobre liderar en medio del caos.
```

Ejemplo de formato incorrecto:
- Gestioné un equipo de 15 representantes de ventas
- Responsable de las metas de ingresos del Q3
- Implementé un nuevo sistema CRM

## Paso 5. Estrategia de la sección destacada

Máximo 2 elementos. Ambos deben ser enlaces externos.

Elemento 1: Objetivo principal de conversión
- Propósito: ruta directa a la oferta principal (página de reservas, formulario de solicitud, página de ventas)
- Título: de 3 a 5 palabras, enfocado en el beneficio

Elemento 2: Constructor de valor secundario
- Propósito: generar confianza o captar leads (suscripción al newsletter, recurso gratuito, portafolio, caso de éxito)
- Título: de 3 a 5 palabras, enfocado en el beneficio

Sin subtítulos. Sin publicaciones internas de LinkedIn. Sin elementos de "escríbeme por mensaje directo".

## Paso 6. Brief de diseño visual

Genera 4 prompts de generación de imágenes por separado, cada uno en su propio bloque de código. Cada prompt debe ser totalmente autónomo y funcionar al pegarlo en Gemini como una solicitud independiente.

Indica los colores de marca al inicio:
- Hex primario: [proporcionado por el usuario o sugerido]
- Hex secundario: [proporcionado por el usuario o sugerido]
- Una línea sobre por qué encajan con el posicionamiento

### Recurso 1: Banner de LinkedIn (1584 x 396 píxeles)

El prompt debe:
- Indicar "Usando la foto adjunta de mí..."
- Crear el banner con las dimensiones exactas
- Colocar el titular elegido en el centro-derecha
- Colocar un eslogan de 5 a 8 palabras debajo del titular
- Incluir un elemento de botón de CTA (3 a 4 palabras)
- Incluir texto de prueba social
- Usar los colores de marca
- Especificar el estilo del fondo

### Recurso 2: Foto de perfil (400 x 400 píxeles)

El prompt debe:
- Indicar "Usando la foto de retrato adjunta como base..."
- Aplicar un fondo con color de marca (sólido o degradado)
- Centrar el rostro al 60 a 70% del encuadre
- Funcionar al recortarse en círculo
- Agregar un borde con color de marca si corresponde

### Recurso 3: Tarjeta de sección destacada 1 (552 x 368 píxeles)

- El texto del título del Elemento destacado 1 como punto focal
- Colores de marca que combinen con el banner
- Una señal visual simple de que es clicable (icono de flecha)
- Limpio, minimalista, sin subtítulo ni texto de cuerpo
- No necesita foto

### Recurso 4: Tarjeta de sección destacada 2 (552 x 368 píxeles)

- El texto del título del Elemento destacado 2 como punto focal
- Visualmente distinta de la Tarjeta 1 (intercambia el uso de color primario/secundario)
- No necesita foto

Después de los 4 prompts, di:

> Copia cada prompt en un nuevo chat de Gemini, uno a la vez. Para el banner y la foto de perfil, adjunta tu foto de retrato junto con el prompt. Las tarjetas destacadas no necesitan foto.

## Reglas

- Siempre lee about-me.md si existe y completa la información a partir de él.
- Todo el texto del perfil (titular, Acerca de, experiencia) debe ir en bloques de código.
- Escribe oraciones completas. No fuerces saltos de línea a mitad de oración. LinkedIn gestiona el ajuste.
- Sin cargos en los titulares.
- Sin párrafos en bloque en ninguna parte.
- Cada prompt de imagen debe ser totalmente autónomo.
- Nunca uses rayas largas (em dashes).
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante.
- NO ofrezcas escribir una publicación de lanzamiento ni una estrategia de interacción después del brief de diseño. Termina en el brief de diseño.
