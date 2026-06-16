---
name: matriz-de-contenido
description: >
  Genera más de 32 ideas de publicaciones de LinkedIn en una sola tabla, combinando los pilares de contenido del usuario con 8 formatos de contenido probados. Basado en la matriz de contenido de Justin Welsh. Usa esta skill siempre que el usuario diga "dame ideas de publicaciones", "matriz de contenido", "sobre qué debería publicar", "genera ideas de publicaciones", "generación de ideas de contenido" o "planifica mi contenido del mes". Toma información de about-me.md y voice.md si existen; de lo contrario, pide los pilares y el contexto.
---

# Matriz de Contenido

## CRÍTICO: Inicio automático al cargar

Cuando esta skill se active, ve directo al Paso 1. No resumas. Comienza a recopilar la información de inmediato.

## Paso 1. Recopilar la información

Revisa el proyecto en busca de about-me.md. Si existe, léelo y completa de antemano la descripción de quién es el usuario. Omite esa pregunta y dile al usuario qué información tomaste.

Si falta about-me.md, pregunta:

> Dame al menos dos párrafos que describan quién eres, a qué te dedicas y de qué te gusta hablar. Mientras más específico seas, más relevantes serán las ideas.

Espera la respuesta.

Luego llama a AskUserQuestion:

```json
[
  {
    "question": "¿Cuáles son tus pilares de contenido?",
    "header": "Pilares",
    "multiSelect": false,
    "options": [
      {"label": "Los escribiré yo", "description": "Tengo de 3 a 4 pilares de contenido para usar"},
      {"label": "Tomar de voice.md", "description": "Usar los temas ya definidos en mis archivos de voz"},
      {"label": "Sugiérelos por mí", "description": "Basándote en mi about-me.md, recomienda 4 pilares"}
    ]
  }
]
```

Si el usuario escribe los suyos, acepta de 3 a 5 pilares. Si son menos de 3, pide más.

Si el usuario elige "Sugiérelos por mí", lee about-me.md, propón 4 pilares que cubran su posicionamiento y pídele que confirme o edite antes de continuar.

## Paso 2. Construir la matriz

Genera una tabla en markdown con:

- Eje X (columnas): 8 formatos de contenido, siempre en este orden:
  1. Accionable
  2. Motivacional
  3. Analítico
  4. Contrario
  5. Observación
  6. X vs Y
  7. Presente vs Futuro
  8. Listicle
- Eje Y (filas): los 3 a 5 pilares del usuario

Cada celda contiene una idea de publicación específica y concreta, adaptada al pilar y al formato. Nada genérico. No reutilizable entre pilares.

Definiciones de formato que debes aplicar al llenar cada celda:

- **Accionable**: Un cómo hacer ultraespecífico. Le enseña al lector a hacer una sola cosa.
- **Motivacional**: Historia inspiradora sobre alguien que hizo algo extraordinario en el nicho.
- **Analítico**: Desglose de por qué algo funciona de la manera en que funciona.
- **Contrario**: Ve en contra del consejo común del nicho y respáldalo.
- **Observación**: Una tendencia oculta, silenciosa o poco comentada que el usuario ha notado.
- **X vs Y**: Compara dos entidades (herramientas, estilos, frameworks, empresas).
- **Presente vs Futuro**: Estado actual frente a una predicción específica, con el porqué.
- **Listicle**: Una lista de recursos, consejos, errores, lecciones o pasos.

La idea de cada celda debe ser un titular específico, no un tema. Bien: "La fórmula de gancho de 3 líneas que le robé a David Ogilvy". Mal: "Ganchos".

## Paso 3. Salida (según la superficie)

Elige el modo de salida según la superficie en la que estés ejecutando. No muestres la tabla en un bloque de código markdown cercado, porque se renderiza como texto plano monoespaciado y hace que una cuadrícula de 5×8 sea difícil de leer.

- **Claude.ai o Claude Cowork (superficies de chat con soporte para gráficos interactivos):** renderiza la matriz como un gráfico interactivo / widget de tabla interactiva. Pilares como filas, formatos como columnas, cada celda con un titular específico. El usuario debe poder hacer clic en una celda para ver el titular completo y cualquier nota de expansión. No vuelques además la tabla como markdown: el gráfico es el entregable.
- **Claude Code (superficie de sistema de archivos, con herramientas Write/Edit):** guarda la matriz en `matriz-de-contenido-YYYY-MM-DD.md` en el directorio de trabajo actual e imprime la misma tabla en línea en la respuesta como una tabla markdown plana (sin envolverla en triple acento grave). Confirma la ruta del archivo para que el usuario pueda abrirlo.
- **Alternativa (sin gráfico interactivo, sin herramientas de sistema de archivos):** muestra una tabla markdown plana en línea. Tampoco la envuelvas en un bloque de código.

Debajo de la tabla o el gráfico, agrega una oración que nombre la única idea más fuerte de toda la matriz y por qué.

## Paso 4. Ofrecer el siguiente paso

Pregunta:

> ¿Hay alguna celda aquí que quieras que escriba como una publicación completa? Refiere la celda por pilar + formato (por ejemplo "Ganchos × Contrario") y la pasaré a la skill redactor-de-publicaciones o formateador-de-publicaciones.

En Claude Code, ofrece también agregar la publicación redactada al mismo archivo `matriz-de-contenido-YYYY-MM-DD.md` bajo la referencia de la celda.

## Reglas

- Mínimo 3 pilares, máximo 5. Más de 5 diluye la matriz.
- Cada idea de celda debe ser específica para ese pilar Y ese formato. No reutilices la misma idea entre pilares.
- Ajusta el lenguaje a la voz del usuario si existe voice.md.
- Español (Latinoamérica) en todo momento, salvo que las muestras sean claramente de otra variante, o salvo que voice.md especifique inglés americano.
- Nunca uses guiones largos (em dashes).
