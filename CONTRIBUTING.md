# Contribuir

Gracias por tu interés en mejorar estas skills. Esta guía cubre cómo agregar nuevas skills, mejorar las existentes y enviar cambios.

## Inicio rápido

1. Haz un fork del repo
2. Crea una rama de funcionalidad (`feat/new-skill-name` o `fix/skill-name-issue`)
3. Haz tus cambios
4. Ejecuta `./validate-skills.sh` para verificar tus cambios frente a la especificación
5. Abre un PR

## Agregar una nueva skill

1. Crea una nueva carpeta dentro de `skills/` con un nombre en minúsculas separado por guiones que coincida con el campo `name` de la skill.
2. Agrega un archivo `SKILL.md` con frontmatter YAML.

Frontmatter mínimo:

```yaml
---
name: my-new-skill
description: >
  One or two sentences explaining what the skill does and when to use it. Include specific trigger phrases the user might say, like "write a post about", "score my draft", or "build me a carousel". The first word of the description should be a verb or action.
---
```

3. Escribe el cuerpo de la skill. La mayoría de las skills siguen esta estructura:

```markdown
# Skill Name

## CRITICAL: Auto-start on load

When this skill triggers, go straight to Step 1. Do not summarise. Start immediately.

## Step 1. Gather inputs
[Use AskUserQuestion where possible]

## Step 2. [Main work]

## Step 3. Output
[Code block showing the exact output format]

## Rules
[Non-negotiables. Voice rules, word limits, what never to do]
```

4. Mantén `SKILL.md` por debajo de las 500 líneas. Mueve el material de referencia a `skills/my-new-skill/references/`.
5. Si la skill tiene plantillas o recursos, colócalos en `skills/my-new-skill/assets/`.
6. Si la skill ejecuta scripts de shell, colócalos en `skills/my-new-skill/scripts/`.

## Mejorar una skill existente

- Mantén el campo name del frontmatter YAML coincidiendo con el nombre de la carpeta.
- No rompas las frases de activación de la skill en la descripción. Otros dependen de ellas.
- Si cambias el formato de salida, actualiza la sección `## Output` en la skill.
- Prueba la skill en tu propio proyecto de Claude antes de abrir un PR.

## Reglas de estilo

Estas reglas aplican a todas las skills del repo:

- Español (Latinoamérica) en todo el texto (sin formas de España como "vosotros")
- Oraciones cortas. Sin guiones largos (em dashes). Sin punto y coma.
- Nunca uses muletillas de marketing vacías: "aprovechar/leverage" (como muletilla), "profundizar a fondo", "desbloquear", "revolucionario", "disruptivo", "rompedor"
- Usa AskUserQuestion para recopilar inputs cuando una llamada a herramienta sea mejor que escribir preguntas
- La sección de reglas al final cubre los aspectos no negociables con expresiones de "never" y "always"
- Cada skill que produce un archivo (como constructor-de-voz que escribe `about-me.md`) indica el nombre de archivo y la ubicación exactos
- Cada skill que depende de la salida de otra skill la verifica antes de ejecutarse

## Convenciones de nombres

- Nombre de la carpeta = campo `name` del YAML = minúsculas, separado por guiones, sin espacios
- Los nombres de skill se leen como verbo-objeto o frases nominales que describen la salida (`redactor-de-publicaciones`, no `writing-posts`)
- Mantén los nombres cortos. Máximo tres palabras siempre que sea posible.

## Probar localmente

Copia tu skill al directorio de skills de Claude:

```bash
cp -r skills/my-new-skill ~/.claude/skills/
```

Luego actívala en una nueva conversación de Claude con las frases listadas en la descripción. Confirma:

- Claude detecta la skill al usar la frase de activación
- Los inputs se recopilan correctamente (AskUserQuestion se renderiza)
- La salida coincide con el formato de la skill
- Todas las dependencias externas (Apify, Gemini, etc.) se verifican antes de usarse

## Enviar un PR

- Título: `feat: add [skill-name]` o `fix: [skill-name] [brief description]`
- Cuerpo: describe qué cambió y por qué, incluye un input/output de ejemplo si es relevante
- Enlaza cualquier issue relacionado

¿Preguntas? Abre un issue en GitHub.
