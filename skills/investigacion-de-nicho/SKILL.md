---
name: investigacion-de-nicho
description: >
  Saca a la luz las 20 historias más relevantes de un nicho de los últimos 7 días usando Claude for Chrome. Fechas verificadas, enlaces reales, ángulos para compartir. Claude maneja el navegador para hacer scroll en Reddit y X y ejecutar búsquedas en Google, exactamente como lo haría un investigador humano. Usa esta skill siempre que el usuario diga "investiga mi nicho", "qué es tendencia", "encuentra historias", "noticias de esta semana", "investigación de contenido", o mencione un nicho y pregunte qué está pasando en él. Requiere que la extensión Claude for Chrome esté habilitada para navegación en vivo.
---

# Investigación de Nicho

## CRÍTICO: Inicio automático al cargar

Cuando esta skill se active, ve directo al Paso 1. No resumas el método de investigación.

## Requisitos previos

Esta skill necesita navegación en vivo. Usa este orden de preferencia:

1. **Extensión Claude for Chrome** (preferido). Verifica que la extensión esté habilitada y que Claude tenga permiso para navegar en la pestaña actual. Si no, dile al usuario:
   > Habilita la extensión Claude for Chrome y abre una pestaña en blanco. Necesito manejar el navegador para hacer scroll en Reddit y X y ejecutar búsquedas en Google con fechas verificadas.
2. **Playwright MCP** como alternativa si la extensión Claude for Chrome no está disponible.
3. **Herramientas WebSearch + WebFetch** como último recurso (menos exhaustivas para el scroll de feeds).

Elige la mejor ruta disponible y continúa.

## Paso 1. Recopilar el nicho

Llama a AskUserQuestion:

```json
[
  {
    "question": "¿Qué nicho quieres investigar?",
    "header": "Nicho",
    "multiSelect": false,
    "options": [
      {"label": "Escribiré mi nicho", "description": "Escribe la frase exacta del nicho después de esto"},
      {"label": "Tomar de about-me.md", "description": "Usar el nicho y la audiencia que ya están en mis archivos de voz"}
    ]
  }
]
```

Si el usuario elige "Tomar de about-me.md", lee el archivo desde la raíz del proyecto. Si el archivo no existe o no nombra un nicho claro, recurre a pedirle al usuario que lo escriba.

## Paso 2. Navegar como un investigador humano

Maneja el navegador a través de estas acciones en orden. Verifica las fechas de publicación de cada elemento. Excluye sin excepción cualquier cosa de más de 7 días desde hoy.

### 2a. Escaneo del feed de Reddit

1. Navega a https://www.reddit.com/ (feed de inicio).
2. Haz scroll en el feed. Carga más publicaciones.
3. Abre publicaciones relevantes para el nicho. En cada publicación, revisa la marca de tiempo de "publicado hace X días".
4. Descarta publicaciones de más de 7 días.
5. Repite con https://www.reddit.com/r/popular/.
6. Busca también cualquier subreddit específico del nicho que aparezca mientras haces scroll.

### 2b. Escaneo del feed de X (Twitter)

1. Navega a https://x.com/home (feed Para ti / For You).
2. Haz scroll por varias pantallas.
3. Abre los hilos completos de los tweets relevantes para el nicho.
4. Revisa la marca de tiempo de la publicación en cada hilo.
5. Descarta publicaciones de más de 7 días, aunque tengan mucha interacción.

### 2c. Búsqueda web en Google

Ejecuta estas búsquedas una por una, abre los primeros resultados, verifica las fechas de publicación.

- `[nicho] news` (configura Herramientas → Cualquier momento → Última semana)
- `[nicho] launch` (última semana)
- `[nicho] controversy` (última semana)
- `[nicho] research` (última semana)
- `[nicho] regulation` (última semana)

Para cada resultado prometedor:

1. Abre la página.
2. Ubica la fecha de publicación visible.
3. Verifica que esté dentro de los últimos 7 días.
4. Si la fecha falta, no está clara o es de más de 7 días, exclúyela.

## Paso 3. Sintetizar en temas

Reúne un grupo amplio de elementos verificados y dentro de la ventana de tiempo. Agrupa los elementos relacionados en temas. Cada tema puede combinar discusión social y cobertura noticiosa.

Selecciona temas que muestren al menos dos de estas características:

- Fuerte atención o discusión
- Desacuerdo o debate claro
- Perspectiva novedosa o información nueva
- Implicaciones reales para el nicho

Apunta a 20 temas. Menos es aceptable si genuinamente hay pocos.

## Paso 4. Salida

Primera línea antes de la tabla:

```
Al [DD/MM/YYYY]
```

Luego una tabla markdown con estas columnas exactas:

```
| Tema / Historia emergente | Plataformas (Reddit, X, Noticias) | Comunidades / Cuentas / Fuentes clave | Enlaces representativos | Señales de atención | Qué está pasando o qué se debate | Por qué importa para [NICHO] | Ángulo para compartir |
```

Sin prosa fuera de la tabla.

## Paso 5. Ofrecer el siguiente paso

Después de la tabla, pregunta:

> ¿Hay alguna fila aquí que quieras que convierta en una publicación de LinkedIn? Llama a la skill redactor-de-publicaciones con el número de fila, o a la skill formateador-de-publicaciones para aplicar un framework.

## Reglas

- Nunca inventes enlaces, métricas ni fechas.
- Excluye sin excepción cualquier cosa de más de 7 días.
- Verifica cada fecha de publicación antes de incluir un elemento. Sin atajos.
- Solo la tabla al final. Sin comentarios, sin párrafo de resumen.
- Si menos de 20 temas pasan el filtro, dilo. No rellenes con elementos débiles.
- Si Claude for Chrome no está disponible y ni Playwright MCP ni WebSearch pueden cubrir bien el scroll de los feeds (Reddit y X), dile al usuario qué falta en lugar de fingir el escaneo.
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante. Formato de fecha DD/MM/YYYY.
- Nunca uses guiones largos (em dashes).
